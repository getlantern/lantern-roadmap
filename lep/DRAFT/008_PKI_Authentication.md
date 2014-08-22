# LEP 008 - PKI-based Authentication

Date:   August 22, 2014

Status: Draft

Author: Ox Cart

## Background

Lantern relies on a network of people who trust each other (trust network) in
order to distribute information about available proxies in a way that makes it
hard for censors to discover them (see [Kaleidoscope]).  Users are identified
and trusted on the basis of their email addresses (possession is proof of
identity).

In order to relieve the burden of having to share this information manually,
Lantern provides an automated mechanism that allows the Lantern client software
to exchange this information without user intervention.  In order for this to
work, Lantern clients must authenticate with the Lantern cloud infrastructure
so that:

1. The client can supply the trust relationships to the Lantern cloud

2. The Lantern cloud can enforce the trust relationships based on whose Lantern
   is trying to communicate with whom

At the moment, we use Google Sign In to do this.  We have consistently received
negative feedback from users who are unsure of why Google is required to use
Lantern, who may mistrust Google itself, or who may be hesitant to give Lantern
access to their Google account.

The purpose of this LEP is to outline an alternate authentication approach that
doesn't involve Google.

## Proposal

The authentication that is required is done by the Lantern client software that
users install on their machines.  Lantern does not provide any purely web-based
services to which users ever need to sign in.

The Lantern client software is able to maintain its own configuration on disk,
consequently it is not necessary to issue any human-readable/usable credentials
to our users.

All that's really required is to tie the Lantern client to an email address that
is in the user's possession, at which point the Lantern client can start
operating under the identity of that email address.

[PKI] as used in the TLS protocol provides a way to do this.

Brave New Software would operate a centralized certificate authority (CA), which
we'll call "janus", that accepts certificate signing requests (CSRs) from
anyone.  This CA would use a well-guarded and relatively short-lived private
key.

When a user signs in using a Lantern client, she simply enters her email
address. The Lantern client generates a CSR using its own private key,
populating the email address as a [Subject Alternative Name], or SAN.  The
Lantern client then submits this CSR to janus.

When janus receives a CSR, it verifies that the CSR contains an email address
SAN, does not contain a [Subject] and does not contain any other SANs.  If the
certificate meets these criteria, janus issues a certificate and sends an email
to the email address indicated in the SAN.  That email includes:

1. A link to the local Lantern client that encodes the certificate as a URL
   (e.g. `http://localhost:15342/AddCert/<PEM Encoded CERT>`)

2. The certificate as an attachment

3. Instructions to click the link or save the attachment and import it into
   Lantern

If Lantern is running, clicking the link will automatically import the
certificate, at which point the sign-in process is complete.  As part of the
sign in screens, the Lantern client will also provide an option to upload a
certificate from disk.

Now, when the Lantern client communicates with Lantern cloud services like
kscope using TLS, it identifies itself by supplying its certificate as a client
certificate. Since this certificate contains the user's email address as the
SAN, Lantern cloud services know whose behalf the client is working on.

![Sequence Diagram](http://www.websequencediagrams.com/cgi-bin/cdraw?lz=dGl0bGUgU2lnbiBJbiBhbmQgQXV0aGVudGljYXRpb24KClVzZXIgLT4gTGFudGVybjoAJQgKAAoHAA8NQ3JlYXRlIFByaXZhdGUgS2V5AAscQ1NSIHcvIEVtYWlsIFNBTgBHDGphbnVzOiBDU1IKAAYFAAkLVmFsaWQAOQcAEwoAQQU6IExpbmsgd2l0aCBlbWJlZGRlZCBjZXJ0aWZpY2F0ZQCBRQkAJwdDbGljayBvbiBsaW5rCgCBAwYAgWAMT3BlbiBVUkwAgVgVU2F2ZSBDAFILAIIHDGtzY29wZTogVExTIGNvbm5lY3QAgREGAIECDAAfBgAjDElkZW50aWZ5IHVzZXIgYnkAghILABsSRG8gd29yayAuLi4&s=vs2010)

[Source Here](http://www.websequencediagrams.com/?lz=dGl0bGUgU2lnbiBJbiBhbmQgQXV0aGVudGljYXRpb24KClVzZXIgLT4gTGFudGVybjoAJQgKAAoHAA8NQ3JlYXRlIFByaXZhdGUgS2V5AAscQ1NSIHcvIEVtYWlsIFNBTgBHDGphbnVzOiBDU1IKAAYFAAkLVmFsaWQAOQcAEwoAQQU6IExpbmsgd2l0aCBlbWJlZGRlZCBjZXJ0aWZpY2F0ZQCBRQkAJwdDbGljayBvbiBsaW5rCgCBAwYAgWAMT3BlbiBVUkwAgVgVU2F2ZSBDAFILAIIHDGtzY29wZTogVExTIGNvbm5lY3QAgREGAIECDAAfBgAjDElkZW50aWZ5IHVzZXIgYnkAghILABsSRG8gd29yayAuLi4&s=vs2010)

[Kaleidoscope]: http://kscope.news.cs.nyu.edu/pub/TR-2008-918.pdf

[PKI]: http://en.wikipedia.org/wiki/Public_key_infrastructure

[Subject Alternative Name]: http://tools.ietf.org/html/rfc2459#section-4.2.1.7

[Subject]: http://tools.ietf.org/html/rfc2459#section-4.1.2.6