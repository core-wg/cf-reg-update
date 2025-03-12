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
  RFC9110: http-sema
  RFC9193: senml-ct
  BCP26:
    -: iana-cons
    =: RFC8126
  IANA.core-parameters:
  IANA.media-types:
  IANA.http-parameters:
  IANA.provisional-standard-media-types:

informative:
  Err4954: 7252

entity:
  SELF: "RFCthis"

--- abstract

This document updates RFC7252 regarding the registration procedures for the "CoAP Content-Formats" IANA registry, within the "Constrained RESTful Environments (CoRE) Parameters" registry group.
This document also introduces a new column, "Media Type", to the registry.

--- middle

# Introduction

{{Section 12.3 of -coap}} describes the registration procedures for the "CoAP Content-Formats" IANA registry within the "Constrained RESTful Environments (CoRE) Parameters" registry group {{IANA.core-parameters}}.
(Note that the columns of this registry have been revised according to {{Err4954}}.)

In particular, the text defines the rules for obtaining CoAP Content-Format identifiers from the "IETF Review" or "IESG Approval" range of the registry (256-9999) as well as from the First Come First Served (FCFS) range of the registry (10000-64999).
For the FCFS range, these rules do not involve the Designated Expert (DE) and are managed solely by IANA personnel to finalize the registration.

Unfortunately, the instructions do not explicitly require checking that the combination of content-type (i.e., media type with optional parameters) and content coding associated with the requested CoAP Content-Format is semantically valid.
This task is generally non-trivial, requires knowledge from multiple documents and technologies, and should not be solely demanded from the registrar.
This lack of guidance may engender confusion in both the registering party and the registrar, and has already led to erroneous registrations.

In {{iana}}, this document updates {{-coap}} by modifying the registration procedures for the "CoAP Content-Formats" registry to mitigate the risk of unintentional or malicious errors.
These updates amend the different ranges of the registry, introduce a review procedure to be performed for most ranges of the registry, and allow the registration of temporary Content-Format identifiers.
This document also introduces a new column, "Media Type", to the registry.

# Conventions and Definitions

This document uses the terms "media type", "content coding", "content-type", and "content format" as defined in {{Section 2 of -senml-ct}}.

# Examples for Erroneous Registrations

This section contains examples of registration requests for a CoAP Content-Format with identifier 64999 in the FCFS range of the "CoAP Content-Formats" registry, as defined in {{Section 12.3 of -coap}} and revised according to {{Err4954}}, which must not be allowed to succeed.

For each of the following example registration requests, one can create a similar instance where the requested registration is for a CoAP Content-Format identifier within the "IETF Review" or "IESG Approval" range of the registry.
Similarly, such registrations must not be allowed to succeed.

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
| application/cose;unknown-parameter=1 | - | 64999 |
{: align="left" title="Attempt at Registering Content-Format for Media Type with Unknown Parameter"}

## The Media Type Parameter Value is Invalid

The registrant requests an FCFS Content-Format ID for an existing media type with an invalid parameter value:

| Content Type | Content Coding | ID |
|--|--|--|
| application/cose;cose-type=invalid | - | 64999 |
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
| application/my;parameter=default | - | 64999 |
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

The registrant requests an FCFS Content-Format ID for a media type that includes a parameter.
The value of this parameter appears distinct from that of a (hypothetical) previously registered Content-Format ID 64900 that also includes this parameter.
However, the semantics of the parameter value are identical to the existing registration.

In this example, the `eat_profile` parameter value (which can be any URI) is set as a Uniform Resource Name (URN) {{?RFC8141}}.
Since for URNs, the Namespace Identifier (`foo` in the example) is defined as case insensitive, the two registrations are semantically identical.

| Content Type | Content Coding | ID |
|--|--|--|
| application/eat+cwt;eat_profile="urn:foo:1" | - | 64900 |
| application/eat+cwt;eat_profile="urn:FOO:1" | - | 64999 |
{: align="left" title="Attempt at Registering an Equivalent Logical Entry with a Different Content-Format ID (3)"}

# Updates to RFC 7252 {#updates}

This section updates {{Section 12.3 of -coap}} and introduces four new “virtual” subsections, 12.3.1 to 12.3.4.

## Updates to Section 12.3 "CoAP Content-Formats Registry" {#iana}

[^replace-self]

The CoAP Content-Formats registration procedures defined in {{Section 12.3 of -coap}} are modified as shown in {{tbl-new-cf-proc}}.

| Range | Registration Procedures | Notes |
|--------|--------|--------|
| 0-255 | Expert Review | Review procedure described in {{&SELF}}, {{checks}} |
| 256-9999 | IETF Review with Expert Review or IESG Approval with Expert Review | Review procedure described in {{&SELF}}, {{checks}} |
| 10000-64999 | Expert Review or First Come First Served | Review procedure described in {{&SELF}}, {{checks}}. <br><br>FCFS is allowed if the registration: <br> * has no parameters, and <br> * has an empty Content Coding, and <br> * the media type is not yet used in this registry, and <br> * the media type is registered (or approved for registration) in the "Media Types" registry {{IANA.media-types}}. <br><br>Expert Review is required if the registration: <br> * includes parameters, and/or <br> * includes a Content Coding, and/or <br> * the media type appears in this registry. |
| 65000-65535 | Experimental Use | No operational use
{: #tbl-new-cf-proc title="Updated CoAP Content-Formats Registration Procedures"}

The 256-9999 range now has registration procedures requiring "IETF Review with Expert Review" or "IESG Approval with Expert Review." In particular:

* All assignments according to "IETF Review with Expert Review" are made on an "IETF Review" basis per {{Section 4.8 of -iana-cons}} with "Expert Review" additionally required per {{Section 4.5 of -iana-cons}}.

  The procedure for early IANA allocation of "standards track code points" defined in {{-iana-early}} also applies. When such a procedure is used, IANA will ask the Designated Expert(s) to approve the early allocation before registration. In addition, working group chairs are encouraged to consult the Expert(s) early during the process outlined in {{Section 3.1 of -iana-early}}.

* All assignments according to "IESG Approval with Expert Review" are made on an "IESG Approval" basis per {{Section 4.10 of -iana-cons}} with "Expert Review" additionally required per {{Section 4.5 of -iana-cons}}.

The 10000-64999 range now has two separate registration procedures.
If the registration consists solely of a registered media type name in the "Content Type" field, without any parameter names or "Content Coding", and the media type has not yet been used in this registry, then the policy is FCFS, as before.
In all other cases, the policy is "Expert Review," following the procedure described in {{checks}}.

A new column with the title "Notes" has been added to the CoAP Content-Formats Registration Procedures shown in {{tbl-new-cf-proc}}.

## New Section 12.3.1 "Temporary Content-Format Registrations"

This section clarifies that the "CoAP Content-Formats" registry allows temporary registrations within the 0-255, 256-9999, and 10000-64999 ranges.

A temporary registration may be created for example by an IANA early allocation action, as requested by the authors of an Internet-Draft in the IETF stream.
If the referenced media type is provisional (that is, included in the IANA "Provisional Standard Media Type" registry {{IANA.provisional-standard-media-types}}) then a created registration is always temporary.

A temporary registration is marked as such by IANA in the corresponding registry entry.
Once the required registration procedure (defined in {{tbl-new-cf-proc}}) for the temporary ID has successfully completed, and the referenced media type is included in the IANA Media Types registry {{IANA.media-types}}, IANA must remove any indication about the temporary nature of the registration so that the entry becomes permanent.

If a temporary registration does not successfully complete the registration procedure, IANA must remove the entry again and set the Content-Format ID value back to "Unassigned".
This may happen for example when an Internet-Draft requesting a Content-Format ID is abandoned.
If a temporary registration (in any range) refers to a provisional media type that is abandoned, IANA must remove the entry and set the Content-Format ID value back to "Unassigned".

Note that in the 10000-64999 range the abandonment of a document requesting a Content-Format ID does not impact the decision whether or not to remove the entry.
That is because the required registration procedure for this range does not require completion of any standards process, nor does it require a registering document.

## New Section 12.3.2 "Adding the Media Type Column to the Registry"

To assist users of the "CoAP Content-Formats" registry in finding detailed information about the media type associated with each CoAP Content-Format, and to ensure that a media type exists before a new entry can be registered, IANA is requested to add a new column "Media Type" to the registry.
This new column is placed directly to the right of the existing "Content Type" column.

The "Media Type" field for each entry lists the (base) media type name and provides a hyperlink to registration information for that media type as recorded by IANA.
If the media type is provisional, the hyperlink points to the IANA "Provisional Standard Media Type" registry {{IANA.provisional-standard-media-types}}.
If a provisional media type becomes a permanent media type, IANA must update the "Media Type" field in the associated registry entries to ensure the hyperlink directs to the registration information for that media type.

Note that the registration request procedure remains unchanged. A requester does not need to fill out the "Media Type" field separately, as the necessary information is already provided in the "Content Type" field of the request.

## New Section 12.3.3 "Expert Review Procedure" {#checks}

The Designated Expert (DE) is instructed to perform the Expert Review, as described by the following checklist:

1. The combination of content-type and content coding for which the registration is requested must not be already present in the "CoAP Content-Formats" registry;
1. The media type associated with the requested Content-Format must either be registered in the "Media Types" registry {{IANA.media-types}} or approved for registration. Alternatively, it may be listed in the "Provisional Standard Media Type" registry {{IANA.provisional-standard-media-types}}. The use of provisional standard media types is only permitted for Content-Format identifiers within the ranges of 0-255 and 256-9999;
1. The optional parameter names must have been defined in association with the media type, and any parameter values associated with such parameter names must be as permitted;
1. The Content Type must be in the preferred format defined in {{preferred-format}};
1. If a Content Coding is specified, it must exist (or must have been approved for registration) in the "HTTP Content Coding" registry of the "Hypertext Transfer Protocol (HTTP) Parameters" {{IANA.http-parameters}}.

For the 0-255 range, in addition to the checks described above, the DE is instructed to also evaluate the requested codepoint concerning the limited availability of the 1-byte codepoint space.
For the 256-9999 range and the 10000-64999 range, a similar criterion may also apply where combinations of media type parameters and content coding choices consume considerable codepoint space.

## New Section 12.3.4 "Preferred Format for the Content Type Field" {#preferred-format}

This section defines the preferred string format for including a requested Content Type into the "CoAP Content-Formats" registry.
During the review process, the Designated Expert(s) or IANA may rewrite a requested Content Type into this preferred string format before approval.

The preferred string format is as defined in {{Section 8.3.1 of -http-sema}} and follows these rules:

1. For any case-insensitive elements, lowercase characters are used.
1. Parameter values are only quoted if the value is such that it requires use of `quoted-string` per {{Section 5.6.6 of -http-sema}}.
   Otherwise, a parameter value is included unquoted.
1. A single semicolon character without any adjacent whitespace characters is used as the separator between media type and parameters.

# Security Considerations

This document hardens the registration procedures of CoAP Content-Formats in ways that reduce the chances of malicious manipulation of the associated registry.

Other than that, it does not change the Security Considerations of {{-coap}}.

# IANA Considerations

This document updates the IANA procedures defined in {{-coap}} for registering CoAP Content-Formats as described in {{updates}}.

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
Christer Holmberg,
Francesca Palombini,
and
Marco Tiloca
for your reviews, comments, suggestions, and fixes.

[^replace-self]: RFC Editor: in this section, please replace {{&SELF}} with the RFC number assigned to this document and remove this note.
