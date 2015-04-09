#LEP 0013 - Proximity Based Peer Selection And Using Nginx for Direct Proxying

Date: April 9, 2015

Status: Draft

Author: atavism

## Abstract

A primary consideration for selecting servers to proxy traffic through is accessibility: only candidate nodes with optimal, unblocked techniques should be chosen. Beyond this assessment though, another crucial factor that emerges is real-world proximity: peers should be choosen according to how close they are to a given client. Any reduction in latency is going to result in shorter delays servicing requests. Since flashlight clients are able to geolocate themselves, this LEP proposes a solution for dynamically servicing requests by accessibility and distance.

## Proposal

Nginx would act as a location-based front-end proxy service. Nginx is configured with the GeoIP module. 

1. A request arrives for a specified resource
2. Nginx checks whether or not the client should be redirected to a closer flashlight server; if so, 
   it rewrites the request to the alternative flashlight server pool
3. Otherwise, forward the request to the specified server and await a response
4. Returns the response to the original requester

Each flashlight server would only act as a direct proxy if that optimal technique is available. Nginx is configured to use the GeoIP module, which uses the MaxMind GeoIP database that contains a list of IP address to location mappings. This table contains a list of approximate locations. We might improve the accuracy of this table by combining data from different geolocation service providers.

Every server contains a virtual host directive that specifies a list of location-based subdomains. When a client first asks a server to proxy traffic, a conditional checkes whether or not the client should be redirected to another, closer server. The client is repeatedly redirected until it eventually ends up in an ideal server pool.

## nginx virtual host configuration
```
map $geoip_city_continent_code $closest_server {
  default www.example.com;
  EU      eu.example.com;
  AS      as.example.com;
}

server {
  server_name example.com
              www.example.com
              eu.example.com
              as.example.com;

  if ($closest_server != $host) {
    rewrite ^ $scheme://$closest_server$request_uri break;
  }

  ...
} [1]
```

Every nginx instance would effectively be its own geoserver that maintains a GeoIP database. As the MaxMind database changes on a weekly basis, we have to periodically synchronize updates. An update server could be used to check for diffs by downloading the latest binary from MaxMind. Any time a diff is detected, those changes are propagated to every flashlight server.

For Iran, where a lot of IP address ranges are missing from the MaxMind database in particular, a fallback mechansim we employ checks whether a given client IP address is contained in an ISP block in the Project Anita database. 

Nginx mitigates the problem of TLS handshake and HTTP response fingerprintability due to its privacy enhancing nature. Lantern network activity would subsequently camouflage better with standard responses that look like typical, ordinary traffic.

Using nginx for direct proxies brings additional security and performance improvements. A configuration file [2] with these options is presented below:

```
# don't send the nginx version number in error pages and Server header
server_tokens off;

# config to don't allow the browser to render the page inside an frame or iframe
# and avoid clickjacking http://en.wikipedia.org/wiki/Clickjacking
# if you need to allow [i]frames, you can use SAMEORIGIN or even set an uri with ALLOW-FROM uri
# https://developer.mozilla.org/en-US/docs/HTTP/X-Frame-Options
add_header X-Frame-Options SAMEORIGIN;

# when serving user-supplied content, include a X-Content-Type-Options: nosniff header along with the Content-Type: header,
# to disable content-type sniffing on some browsers.
# https://www.owasp.org/index.php/List_of_useful_HTTP_headers
# currently suppoorted in IE > 8 http://blogs.msdn.com/b/ie/archive/2008/09/02/ie8-security-part-vi-beta-2-update.aspx
# http://msdn.microsoft.com/en-us/library/ie/gg622941(v=vs.85).aspx
# 'soon' on Firefox https://bugzilla.mozilla.org/show_bug.cgi?id=471020
add_header X-Content-Type-Options nosniff;

# This header enables the Cross-site scripting (XSS) filter built into most recent web browsers.
# It's usually enabled by default anyway, so the role of this header is to re-enable the filter for 
# this particular website if it was disabled by the user.
# https://www.owasp.org/index.php/List_of_useful_HTTP_headers
add_header X-XSS-Protection "1; mode=block";

# with Content Security Policy (CSP) enabled(and a browser that supports it(http://caniuse.com/#feat=contentsecuritypolicy),
# you can tell the browser that it can only download content from the domains you explicitly allow
# http://www.html5rocks.com/en/tutorials/security/content-security-policy/
# https://www.owasp.org/index.php/Content_Security_Policy
# I need to change our application code so we can increase security by disabling 'unsafe-inline' 'unsafe-eval'
# directives for css and js(if you have inline css or js, you will need to keep it too).
# more: http://www.html5rocks.com/en/tutorials/security/content-security-policy/#inline-code-considered-harmful
add_header Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval' https://ssl.google-analytics.com https://assets.zendesk.com https://connect.facebook.net; img-src 'self' https://ssl.google-analytics.com https://s-static.ak.facebook.com https://assets.zendesk.com; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com https://assets.zendesk.com; font-src 'self' https://themes.googleusercontent.com; frame-src https://assets.zendesk.com https://www.facebook.com https://s-static.ak.facebook.com https://tautt.zendesk.com; object-src 'none'";

server {
  listen 443 ssl default deferred;
  server_name .forgott.com;

  ssl_certificate /etc/nginx/ssl/star_forgott_com.crt;
  ssl_certificate_key /etc/nginx/ssl/star_forgott_com.key;

  # enable session resumption to improve https performance
  # http://vincent.bernat.im/en/blog/2011-ssl-session-reuse-rfc5077.html
  ssl_session_cache shared:SSL:50m;
  ssl_session_timeout 5m;

  # Diffie-Hellman parameter for DHE ciphersuites, recommended 2048 bits
  ssl_dhparam /etc/nginx/ssl/dhparam.pem;

  # enables server-side protection from BEAST attacks
  # http://blog.ivanristic.com/2013/09/is-beast-still-a-threat.html
  ssl_prefer_server_ciphers on;
  # disable SSLv3(enabled by default since nginx 0.8.19) since it's less secure then TLS http://en.wikipedia.org/wiki/Secure_Sockets_Layer#SSL_3.0
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  # ciphers chosen for forward secrecy and compatibility
  # http://blog.ivanristic.com/2013/08/configuring-apache-nginx-and-openssl-for-forward-secrecy.html
  ssl_ciphers "ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:DES-CBC3-SHA:HIGH:!aNULL:!eNULL:!EXPORT:!DES:!MD5:!PSK:!RC4";

  # enable ocsp stapling (mechanism by which a site can convey certificate revocation information to visitors in a privacy-preserving, scalable manner)
  # http://blog.mozilla.org/security/2013/07/29/ocsp-stapling-in-firefox/
  resolver 8.8.8.8;
  ssl_stapling on;
  ssl_trusted_certificate /etc/nginx/ssl/star_forgott_com.crt;

  # config to enable HSTS(HTTP Strict Transport Security) https://developer.mozilla.org/en-US/docs/Security/HTTP_Strict_Transport_Security
  # to avoid ssl stripping https://en.wikipedia.org/wiki/SSL_stripping#SSL_stripping
  add_header Strict-Transport-Security "max-age=31536000; includeSubdomains;";

  # ... the rest of your configuration
}

# redirect all http traffic to https
server {
  listen 80;
  server_name .forgott.com;
  return 301 https://$host$request_uri;
}
```

## Implementation Details

## References
[1] https://www.digitalocean.com/community/tutorials/how-to-use-nginx-as-a-global-traffic-director-on-debian-or-ubuntu
[2] https://gist.github.com/plentz/6737338 and http://tautt.com/best-nginx-configuration-for-security/
