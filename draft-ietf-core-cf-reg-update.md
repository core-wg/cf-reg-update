---
title: "Update to the IANA CoAP Content-Formats Registration Procedures"
abbrev: "CoAP Content-Format Registrations Update"
category: std

docname: draft-ietf-core-cf-reg-update-latest
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
  github: "core-wg/cf-reg-update"
  latest: "https://core-wg.github.io/cf-reg-update/draft-ietf-core-cf-reg-update.html"

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
  IANA.core-parameters:
  IANA.media-types:
  IANA.http-parameters:

entity:
  SELF: "RFCthis"

--- abstract

This document updates RFC7252 regarding the registration procedures for the "CoAP Content-Formats" registry, within the "CoRE Parameters" registry group,
specifically, those regarding the First Come First Served (FCFS) portion of the registry.

--- middle

# Introduction

{{Section 12.3 of -coap}} describes the registration procedures for the "CoAP Content-Formats" registry within the "CoRE Parameters" registry group {{IANA.core-parameters}}.
(Note that the columns of this registry have been revised according to {{Err4954}}.)
In particular, the text defines the rules for obtaining CoAP Content-Format identifiers from the First Come First Served (FCFS) portion of the registry (10000-64999).
These rules do not involve the Designated Expert (DE) and are managed solely by IANA personnel to finalize the registration.
Unfortunately, the instructions do not explicitly require checking that the combination of content-type (i.e., media type with optional parameters) and content coding associated with the requested CoAP Content-Format is semantically valid.
This task is generally non-trivial, requiring knowledge from multiple documents and technologies, which is not a task to demand solely from the registrar.
This lack of guidance may engender confusion in both the registering party and the registrar, and has already led to erroneous registrations.

In {{iana}}, this document updates {{-coap}} by modifying the registration procedures for the "CoAP Content-Formats" registry regarding its FCFS portion, to mitigate the risk of unintentional or malicious errors.

# Conventions and Definitions

{::boilerplate bcp14-tagged-bcp14}

This document uses the terms "media type", "content coding", "content-type" and "content format" as defined in {{Section 2 of -senml-cf}}.

# Examples for Erroneous Registrations

This section contains a few examples of registration requests for a CoAP Content-Format with identifier 64999 in the FCFS space that must not be allowed to succeed.

## The Media Type is Unknown {#ex-unknown-mt}

The registrant requests an FCFS Content-Format ID for an unknown media type:

| Content Type | Content Coding | ID |
|--|--|--|
| application/unknown+cbor | - | 64999 |
{: align="left" title="Attempt at Registering Content-Format for an Unknown Media Type"}

## The Media Type Parameter is Unknown

The registrant requests an FCFS Content-Format ID for an existing media type with an unknown parameter:

| Content Type | Content Coding | ID |
|--|--|--|
| application/cose; unknown-parameter=1 | - | 64999 |
{: align="left" title="Attempt at Registering Content-Format for Media Type with Unknown Parameter"}

## The Media Type Parameter Value is Invalid

The registrant requests an FCFS Content-Format ID for an existing media type with an invalid parameter value:

| Content Type | Content Coding | ID |
|--|--|--|
| application/cose; cose-type=invalid | - | 64999 |
{: align="left" title="Attempt at Registering Content-Format for Media Type with Invalid Parameter Value"}

## The Content Coding is Unknown

The registrant requests an FCFS Content-Format ID for an existing media type with an unknown content coding:

| Content Type | Content Coding | ID |
|--|--|--|
| application/senml+cbor | inflate | 64999 |
{: align="left" title="Attempt at Registering Content-Format with Unknown Content Coding"}

## Duplicate Entry with Default Media Type Parameters

The registrant requests an FCFS Content-Format ID for a media type that includes a parameter set to its default value, while
a (hypothetical) Content-Format ID 64900 is already registered for this media type without that parameter.
As a result, this could lead to the creation of two separate Content-Format IDs for the same "logical" entry.

| Content Type | Content Coding | ID |
|--|--|--|
| application/my | - | 64900 |
| application/my; parameter=default | - | 64999 |
{: align="left" title="Attempt at Registering an Equivalent Logical Entry with a Different Content-Format ID (1)"}

## Duplicate Entry with Default Content Coding

The registrant requests an FCFS Content-Format ID for the "identity" Content Coding, which is the default coding.
If accepted, this request would duplicate an entry with (hypothetical)
Content-Format ID 64900 where the "Content Coding" field is left empty.

| Content Type | Content Coding | ID |
|--|--|--|
| application/my | - | 64900 |
| application/my | identity | 64999 |
{: align="left" title="Attempt at Registering an Equivalent Logical Entry with a Different Content-Format ID (2)"}

## Duplicate Entry with Equivalent Parameter

The registrant requests an FCFS Content-Format ID for a Content Type with a parameter that has a value that looks different (based on the string) from a previously registered Content-Format that includes this parameter also.
However, the semantics of the parameter value are identical to the existing registration.

In this example, the `eat_profile` parameter value (which can be any URI) is set as a Uniform Resource Name (URN) {{RFC8141}}.
And for URNs, the Namespace Identifier (`foo` in the example) is defined as case insensitive.
Therefore the two registrations are semantically identical.

| Content Type | Content Coding | ID |
|--|--|--|
| application/eat+cwt; eat_profile="urn:foo:1" | - | 64900 |
| application/eat+cwt; eat_profile="urn:FOO:1" | - | 64999 |
{: align="left" title="Attempt at Registering an Equivalent Logical Entry with a Different Content-Format ID (3)"}


# Security Considerations

This document hardens the registration procedures of CoAP Content-Formats in ways that reduce the chances of malicious manipulation of the associated registry.

Other than that, it does not change the Security Considerations of {{-coap}}.

# IANA Considerations {#iana}

[^replace-self]

The CoAP Content-Formats registration procedures defined in {{Section 12.3 of -coap}} are modified as shown in {{tbl-new-cf-proc}}.

| Range | Registration Procedures | Note |
|--------|--------|
| 0-255 | Expert Review | Full review described in {{&SELF}}, {{full-checks}} |
| 256-9999 | IETF Review or IESG Approval | |
| 10000-64999 (No parameters and empty Content Coding and media type not yet used in this registry) | First Come First Served | The corresponding media type must be registered (or approved for publication) in the "Media Types" registry {{IANA.media-types}} |
| 10000-64999 (Includes parameters and/or Content Coding and/or media type appears in this registry) | Expert Review | Lightweight review described in {{&SELF}}, {{checks}} |
| 65000-65535 | Experimental use (no operational use) |
{: #tbl-new-cf-proc title="Updated CoAP Content-Formats Registration Procedures"}

The 10000-64999 range now has two separate registration procedures.
If the registration consists solely of a registered media type name in the "Content Type" field, without any parameter names or "Content Coding", and the media type has not yet been used in this registry, then the policy is FCFS, as before.
In all other cases, the policy will be Expert Review, following the checklist described in {{checks}}.

A new column with the title "Note" has been added to the registry, which contains information about expected checks.

## "Full" Expert Review Checks {#full-checks}

For the 0-255 range, the DE is instructed to perform a "Full Review" described in this section, not only the "lightweight" Expert Review that may apply to the 10000-64999 range.
For this range, in addition to the checks described in {{checks}}, the DE is instructed to also evaluate the requested codepoint concerning the limited availability of the 1-byte codepoint space.
For the 10000-64999 range, this criterion does not apply.

## "Lightweight" Expert Review Checks {#checks}

For the 10000-64999 range, the Designated Expert is instructed to perform the "lightweight" Expert review, as described by the following checklist:

1. The combination of content-type and content coding for which the registration is requested must not be already present in the "CoAP Content-Formats" registry;
1. The media type associated with the requested Content-Format must exist (or must have been approved for registration) in the "Media Types" registry {{IANA.media-types}};
1. The optional parameter names must have been defined in association with the media type, and any parameter values associated with such parameter names must be as permitted;
1. If a Content Coding is specified, it must exist (or must have been approved for registration) in the "HTTP Content Coding" registry of the "Hypertext Transfer Protocol (HTTP) Parameters" {{IANA.http-parameters}}.

<!-- Should these actually use BCP14 MUSTs? -->

## Temporary Note Removal
{:removeinrfc}

The following note has been added to the registry as a temporary fix:

> "Note: The validity of the combination of Content Coding, Content Type and parameters is checked prior to assignment."

IANA is instructed to remove this note from the registry when this document is approved for publication.
RFC-Editor: please remove this section once the note has been removed.

--- back

# Acknowledgments
{:numbered="false"}

Thank you
Amanda Baber,
Carsten Bormann,
Francesca Palombini,
and
Marco Tiloca
for your reviews, comments, suggestions and fixes.

[^replace-self]: RFC Editor: in this section, please replace {{&SELF}} with the RFC number assigned to this document and remove this note.
