#LEP 009 - Dialing Directly to Masquerade IPs in Flashlight

Date:   October 28, 2014

Status: Draft

Author: aranhoide

## Abstract

In the configuration that ships with Flashlight, as well as in the
configuration that is fed to it through S3, masquerades are addressed only by
domain name, and clients in censored countries obtain their IP through a regular DNS query.  This is vulnerable to DNS poisoning, a type of attack we've seen censors use in other contexts.

This document proposes a MVP solution to mitigate this risk.  More elaborate
solutions are postponed on the grounds that it's not yet clear that censors will refrain from blocking all Cloudflare IPs, or all the ones we're using (this information being public by design), if it's us they're after, especially once we deploy such a scheme to foil plain DNS poisoning.

## Proposal

An `ip` field is added to each item in `flashlight/certs/cloud.yaml:client/masqueradesets/cloudflare`.

An `IP` field is added to the Masquerade struct in
`flashlight/client/config.go`, and hence in each entry in
`flashlight/config/masquerades.go`.

The Python scripts in cert/ are updated to feed the IPs to these files.

The configured IPs are sourced by doing DNS queries from the location where our flashlight servers run (currently Singapore).

The flashlight client dials directly to the IP, doing the verification of the
cert as a separate step. [1]

## Rationale

Other (mostly more elaborate) options were considered and rejected in this
MVP iteration, mainly because of the overarching concern expressed above.  All
but the most egregious performance concerns were discarded too, on the grounds
that as long as we can get the system bootstrapped, we can provide users with
more performant options that don't involve domain fronting at all (e.g. old
fallback proxies).

## Backwards Compatibility

No backwards compatibility issues are expected.  Go's YAML unpacking will
silently ignore any fields that it can't unmarshal to a Go structure, so old clients should just ignore the `ip` field in masquerade entries and keep on using DNS to obtain that as before.


---
[1] Proof-of-concept implementation of this dialing-verification scheme:

```go
package main

import(
    "fmt"
    "crypto/tls"
)

func main() {
    c := tls.Config{
        InsecureSkipVerify: true,
    }
    conn, err := tls.Dial("tcp", "104.16.15.15:443", &c)
    if err != nil {
        fmt.Println("tls.Dial error:", err)
        return
    }
    err = conn.VerifyHostname("jquery.com")
    if err == nil {
        fmt.Println("Aright!")
    } else {
        fmt.Println("VerifyHostname error:", err)
    }
}
```
