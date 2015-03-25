# LEP 012 - Analytics

Date: March 24, 2015

Status: Draft

Author: atavism

## Proposal

Send server-side events to Google Analytics. This will enable us to track user activity and general Lantern usage on the backend prior to forwarding requests to GA.

## Description

Google Analytics is a web analytics service that records website traffic. It's a convenient, well-known system for tracking information about visitors. The Lantern front-end already makes extensive use of it to analyze traffic and user activity. Currently, the type of user information we collect includes browser, operating system, language, service provider, session duration, page views, number of new vs. returning users, etc.

There are numerous backend events it would be useful to track in addition to front-end user activity and installs. Combining these events with existing user data, and being able to inspect it from the GA dashboard, would give us deeper insight into general Lantern usage. For instance, we may want to track certain events such as geolookups, circumvention techniques (domain-fronting, direct proxies, local DNS resolution, etc), config updates, etc.

If a user disables anonymous-usage reporting, we don't track anything about their activity.

## Parameters

Parameter  | GA parameter name | Description
------------- | ------------- | ------------- 
tid           | tid           | Tracking ID
cid           | cid           | Client ID
lid           | -             | Lantern User ID (random token)
ul            | ul            | Language
os            | -             | Operating System
browser       | -             | Browser

The missing GA parameters are still sent to GA as [custom variables](https://developers.google.com/analytics/devguides/collection/gajs/gaTrackingCustomVariables).
 
## User Interaction

The following is a list of user events that are tracked.

Event  | Description
---------------      | -------------
First page view       | User opens the UI for the first time
Access settings      | Settings screen is open
Closes UI            | User closes the UI tab
Zoom                 | Map zoom-level interaction
Adds site            | New proxied sites entry
Removes site         | Proxied sites entry deleted
Access proxied sites | Proxied sites list accessed
Error                | System error encountered
