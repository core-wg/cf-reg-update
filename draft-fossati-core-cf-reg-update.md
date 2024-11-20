---
title: "Update to the IANA CoAP Content-Formats Registration Procedures"
abbrev: "CoAP C-F Registrations Update"
category: std

docname: draft-fossati-core-cf-reg-update-latest
submissiontype: IETF
number:
updates: 7252
date:
consensus: true
v: 3
area: "Web and Internet Transport"
workgroup: "Constrained RESTful Environments"
keyword:
 - IANA
 - registration
 - update
 - CoAP
 - Content-Format
venue:
  group: "Constrained RESTful Environments"
  type: "Working Group"
  mail: "core@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/core/"
  github: "thomas-fossati/draft-cf-reg-update"
  latest: "https://thomas-fossati.github.io/draft-cf-reg-update/draft-fossati-core-cf-reg-update.html"

author:
 - fullname: Thomas Fossati
   organization: Linaro
   email: thomas.fossati@linaro.org
 - fullname: Esko Dijk
   organization: IoTconsultancy.nl
   email: esko.dijk@iotconsultancy.nl

normative:
  RFC7252: coap

informative:
  Err4954: 7252
  RFC9193: senml-cf

--- abstract

This document updates the registration procedures for the "CoAP Content-Formats" registry, within the "CoRE Parameters" registry group, defined in Section 12.3 of RFC7252.
Specifically, those regarding the First Come First Served (FCFS) portion of the registry.

--- middle

# Introduction

{{Section 12.3 of -coap}} describes the registration procedures for the "CoAP Content-Formats" registry within the "CoRE Parameters" registry group {{?IANA.core-parameters}}.
(Note that the columns of this registry have been revised according to {{Err4954}}.)
In particular, the text defines the rules for obtaining CoAP Content-Format identifiers from the First Come First Served (FCFS) portion of the registry (10000-64999).
These rules do not involve the Designated Expert (DE) and are managed solely by IANA personnel to finalize the registration.
Unfortunately, the instructions do not explicitly require checking that the combination of content-type (i.e., media type with optional parameters) and content coding associated with the requested CoAP Content-Format is semantically valid.
This task is generally non-trivial, requiring knowledge from multiple documents and technologies, which is unfair to demand solely from the registrar.
This lack of guidance may engender confusion in both the registering party and the registrar, and could eventually lead to erroneous registrations.

{{iana}} of this memo updates the registration procedures for the "CoAP Content-Formats" registry regarding its FCFS portion to reduce the risk of accidental or malicious errors.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

This document uses the terms "media type", "content coding", "content-type" and "content format" as defined in {{Section 2 of -senml-cf}}.

# (Bad) Examples

This section contains a few examples of registration requests for a CoAP Content-Format with identifier in the FCFS space (64999) that should not be allowed to succeed.

## The Media Type is Unknown {#ex-unknown-mt}

The registrant requests an FCFS C-F ID for an unknown media type:

| Content Type | Content Coding | ID |
|--|--|--|
| application/unknown+cbor | - | 64999 |
{: align="left" title="Attempt at Registering C-F for an Unknown Media Type"}

## The Media Type Parameter is Unknown

The registrant requests an FCFS C-F ID for an existing media type with an unknown parameter:

| Content Type | Content Coding | ID |
|--|--|--|
| application/cose; unknown-parameter=1 | - | 64999 |
{: align="left" title="Attempt at Registering C-F for Media Type with Unknown Parameter"}

## The Media Type Parameter Value is Invalid

The registrant requests an FCFS C-F ID for an existing media type with an invalid parameter value:

| Content Type | Content Coding | ID |
|--|--|--|
| application/cose; cose-type=invalid | - | 64999 |
{: align="left" title="Attempt at Registering C-F for Media Type with Invalid Parameter Value"}

## The Content Coding is Unknown

The registrant requests an FCFS C-F ID for an existing media type with an unknown content coding, "inflate":

| Content Type | Content Coding | ID |
|--|--|--|
| application/senml+cbor | inflate | 64999 |
{: align="left" title="Attempt at Registering C-F with Unknown Content Coding"}

# Security Considerations

This memo hardens the registration procedures of CoAP Content-Formats in ways that reduce the chances of malicious manipulation of the associated registry.

Other than that, it does not change the Security Considerations of {{-coap}}.

# IANA Considerations {#iana}

The CoAP Content-Formats registration procedures defined in {{Section 12.3 of -coap}} are updated as follows:

| Range | Registration Procedures |
|--------|--------|
| 0-255 | Expert Review (Full) |
| 256-9999 | IETF Review or IESG Approval |
| 10000-64999 | Expert Review (Expert Check: FCFS+) |
| 65000-65535 | Experimental use (no operational use) |
{: #tbl-new-cf-proc title="Updated CoAP Content-Formats Registration Procedures"}

The DE checks consist of the following steps:

1. The combination of content-type and content coding for which the registration is requested must not be already present in the "CoAP Content-Formats" registry;
1. The media type associated with the requested Content-Format must exist in the "Media Types" registry {{?IANA.media-types}}, or IANA has approved its registration;
1. The optional parameter names must exist in association with the media type, and any parameter values associated with such parameter names are as expected;
1. If a Content Coding is specified, it must exist in the "HTTP Content Coding Registry" of the "Hypertext Transfer Protocol (HTTP) Parameters" {{?IANA.http-parameters}}, or IANA has approved its registration.

The registration procedure for the 0-255 range has been slightly modified -- from "Expert Review" to "Expert Review (Full)" -- to clearly distinguish it from the new "Expert Review (Expert Check: FCFS+)" policy that applies to the 10000-64999 range.
For the 0-255 range, the DE must also evaluate the requested codepoint concerning the limited availability of the 1-byte codepoint space.
For the 10000-64999 range, this criterion does not apply.

--- back

# Acknowledgments
{:numbered="false"}

Thank you
Carsten Bormann,
Francesca Palombini
and
Marco Tiloca
for your reviews and comments.
