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
  RFC7120: iana-early
  RFC7252: coap
  RFC9110:
  BCP26:
    -: iana-cons
    =: RFC8126

informative:
  Err4954: 7252
  RFC9193: senml-cf
  IANA.core-parameters:
  IANA.media-types:
  IANA.http-parameters:

entity:
  SELF: "RFCthis"

--- abstract

This document updates RFC7252 regarding the registration procedures for the "CoAP Content-Formats" registry, within the "CoRE Parameters" registry group. The affected registration procedures are
specifically those regarding the IETF Review or IESG Approval portion of the registry as well as those regarding the First Come First Served (FCFS) portion of the registry.

--- middle

# Introduction

{{Section 12.3 of -coap}} describes the registration procedures for the "CoAP Content-Formats" registry within the "CoRE Parameters" registry group {{IANA.core-parameters}}.
(Note that the columns of this registry have been revised according to {{Err4954}}.)

In particular, the text defines the rules for obtaining CoAP Content-Format identifiers from the IETF Review or IESG Approval portion of the registry (256-9999) as well as from the First Come First Served (FCFS) portion of the registry (10000-64999).
These rules do not involve the Designated Expert (DE) and are managed solely by IANA personnel to finalize the registration.

Unfortunately, the instructions do not explicitly require checking that the combination of content-type (i.e., media type with optional parameters) and content coding associated with the requested CoAP Content-Format is semantically valid.
This task is generally non-trivial, requiring knowledge from multiple documents and technologies, which is not a task to demand solely from the registrar.
This lack of guidance may engender confusion in both the registering party and the registrar, and has already led to erroneous registrations.

In {{iana}}, this document updates {{-coap}} by modifying the registration procedures for the "CoAP Content-Formats" registry regarding its IETF Review or IESG Approval portion as well as its FCFS portion, to mitigate the risk of unintentional or malicious errors.

# Conventions and Definitions

{::boilerplate bcp14-tagged-bcp14}

This document uses the terms "media type", "content coding", "content-type" and "content format" as defined in {{Section 2 of -senml-cf}}.

# Examples for Erroneous Registrations

This section contains a few examples of registration requests for a CoAP Content-Format with identifier 64999 in the FCFS space that must not be allowed to succeed.

The following considerations also apply to alternative examples where, for the same combination of content type and content coding, a registration was requested for a CoAP Content-Format with identifier in the IETF Review or IESG Approval space. That is, such registrations must not be allowed to succeed.

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

# Security Considerations

This document hardens the registration procedures of CoAP Content-Formats in ways that reduce the chances of malicious manipulation of the associated registry.

Other than that, it does not change the Security Considerations of {{-coap}}.

# IANA Considerations {#iana}

[^replace-self]

The CoAP Content-Formats registration procedures defined in {{Section 12.3 of -coap}} are modified as shown in {{tbl-new-cf-proc}}.

| Range | Registration Procedures | Note |
|--------|--------|
| 0-255 | Expert Review | Review checks described in {{&SELF}}, {{checks}} |
| 256-9999 | IETF Review with Expert Review or IESG Approval with Expert Review | Review checks described in {{&SELF}}, {{checks}} |
| 10000-64999 (No parameters and empty Content Coding and media type not yet used in this registry) | First Come First Served | The corresponding media type must be registered (or approved for publication) in the "Media Types" registry {{IANA.media-types}} |
| 10000-64999 (Includes parameters and/or Content Coding and/or media type appears in this registry) | Expert Review | Review checks described in {{&SELF}}, {{checks}} |
| 65000-65535 | Experimental use (no operational use) |
{: #tbl-new-cf-proc title="Updated CoAP Content-Formats Registration Procedures"}

The 256-9999 range now has registration procedures requiring IETF Review with Expert Review or IESG Approval with Expert Review. In particular:

* All assignments according to "IETF Review with Expert Review" are made on an "IETF Review" basis per {{Section 4.8 of -iana-cons}} with "Expert Review" additionally required per {{Section 4.5 of -iana-cons}}.

  The procedure for early IANA allocation of "standards track code points" defined in {{-iana-early}} also applies. When such a procedure is used, IANA will ask the Designated Expert(s) to approve the early allocation before registration. In addition, working group chairs are encouraged to consult the Expert(s) early during the process outlined in {{Section 3.1 of -iana-early}}.

* All assignments according to "IESG Approval with Expert Review" are made on an "IESG Approval" basis per {{Section 4.10 of -iana-cons}} with "Expert Review" additionally required per {{Section 4.5 of -iana-cons}}.

The 10000-64999 range now has two separate registration procedures.
If the registration consists solely of a registered media type name in the "Content Type" field, without any parameter names or "Content Coding", and the media type has not yet been used in this registry, then the policy is FCFS, as before.
In all other cases, the policy will be Expert Review, following the checklist described in {{checks}}.

A new column with the title "Note" has been added to the registry, which contains information about expected checks.

## Expert Review Checks {#checks}

The Designated Expert is instructed to perform the Expert Review, as described by the following checklist:

1. The combination of content-type and content coding for which the registration is requested must not be already present in the "CoAP Content-Formats" registry;
1. The media type associated with the requested Content-Format must exist (or must have been approved for registration) in the "Media Types" registry {{IANA.media-types}};
1. The optional parameter names must have been defined in association with the media type, and any parameter values associated with such parameter names must be as permitted;
1. The Content Type is in the preferred format defined in {{preferred-format}};
1. If a Content Coding is specified, it must exist (or must have been approved for registration) in the "HTTP Content Coding" registry of the "Hypertext Transfer Protocol (HTTP) Parameters" {{IANA.http-parameters}}.

For the 0-255 range, in addition to the checks described above, the DE is instructed to also evaluate the requested codepoint concerning the limited availability of the 1-byte codepoint space.
For the 256-9999 range and the 10000-64999 range, this criterion does not apply.

<!-- Should these actually use BCP14 MUSTs? -->

## Preferred Format for the Content Type Field {#preferred-format}

This section defines the preferred string format for including a requested Content Type into the CoAP Content-Formats registry.
During the review procedure, the Designated Expert(s) or IANA may rewrite a requested Content Type to this preferred string format prior to approval.

The preferred string format is as follows:

1. For any case-insensitive elements, lowercase characters must be used.
   See {{Section 8.3.1 of RFC9110}} for a definition of case-insensitive elements and some examples.
1. Parameter values are only quoted if the value is such that it requires use of `quoted-string` per {{Section 5.6.6 of RFC9110}}.
   Otherwise, a parameter value is included unquoted.
1. A semicolon followed by one space character is used as the separator between media type and parameters.

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
