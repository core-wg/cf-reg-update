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

normative:
  RFC7252: coap

informative:

--- abstract

This document updates the registration procedures for the "CoAP Content-Formats" sub-registry, within the "CoRE Parameters" registry, defined in Section 12.3 of RFC7252.
Specifically, those regarding the First Come First Served (FCFS) portion of the sub-registry.

--- middle

# Introduction

{{Section 12.3 of -coap}} describes the registration procedures for the "CoAP Content-Formats" sub-registry within the "CoRE Parameters" registry {{?IANA.core-parameters}}.
In particular, it defines the rules for obtaining CoAP Content-Formats from the First Come First Served (FCFS) portion of sub-registry (10000-64999).
Said rules do not require the Designated Expert (DE) to be in the loop, and rely entirely on IANA personnel to complete the registration.
Unfortunately, the instructions do not explicitly require checking that the combination of media type, optional parameters and content coding associated with the requested CoAP Content-Format is semantically valid.
This is, in general, a non-trivial task, involving knowledge across multiple documents and technologies, which is unfair to demand from the registrar alone.
This lack of guidance may engender confusion in both the registering party and the registrar, and could eventually lead to erroneous registrations.

{{iana}} of this memo updates the registration procedures for the "CoAP Content-Formats" sub-registry regarding its FCFS portion to reduce the risk of accidental or malicious errors.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# (Bad) Examples

This section contains a few examples of registration requests for a CoAP Content-Format in the FCFS space (64999) that should not be allowed to succeed.

## The Media Type is Unknown {#ex-unknown-mt}

The registrant requests an FCFS C-F for an unknown media type:

| Content Format | Media Type | Content Coding |
|--|--|--|
| 64999 | application/unknown+cbor | - |
{: align="left" title="Attempt at Registering C-F for an Unknown Media Type"}

## The Media Type Parameter is Unknown

The registrant requests an FCFS C-F for an existing media type with an unknown parameter:

| Content Format | Media Type | Content Coding |
|--|--|--|
| 64999 | application/cose; unknown-parameter=1 | - |
{: align="left" title="Attempt at Registering C-F for Media Type with Unknown Parameter"}

## The Media Type Parameter Value is Invalid

The registrant requests an FCFS C-F for an existing media type with an invalid parameter value:

| Content Format | Media Type | Content Coding |
|--|--|--|
| 64999 | application/cose; cose-type=invalid | - |
{: align="left" title="Attempt at Registering C-F for Media Type with Invalid Parameter Value"}

## The Content Coding is Unknown

The registrant requests an FCFS C-F for an existing media type with an unknown content coding, "inflate":

| Content Format | Media Type | Content Coding |
|--|--|--|
| 64999 | application/senml+cbor | inflate |
{: align="left" title="Attempt at Registering C-F with Unknown Content Coding"}

# Security Considerations

This memo hardens the registration procedures of CoAP Content-Formats in ways that reduce the chances of malicious manipulation of the associated registry.

Other than that, it does not change the Security Considerations of {{-coap}}.

# IANA Considerations {#iana}

Before assigning a new Content-Format in the FCFS space, the registrar MUST check that:

1. The media type associated to the requested Content-Format is not associated to an already registered CoAP Content-Format;
1. The media type associated to the requested Content-Format exists in the "Media Types" registry {{?IANA.media-types}}, or its registration has been approved;
1. The optional parameter names exist in association with the media type, and any parameter values associated with such parameter names are as expected;
1. If a Content Coding is specified, it exists in the "HTTP Content Coding Registry" of the "Hypertext Transfer Protocol (HTTP) Parameters" {{?IANA.http-parameters}}.

The registrar MAY forward the registration of a CoAP Content-Format in the FCFS portion of the registry (i.e., one for a CoAP Content-Format in range 10000-64999) to one or more DEs of the registry for consultation.

The registrar MUST forward the registration to one or more DEs of the registry when such registration involves any media type parameters.

If the request is forwarded to the DE, their judgement is binding.

--- back
