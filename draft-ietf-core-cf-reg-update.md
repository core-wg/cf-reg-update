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
Furthermore, this document reserves Content-Format identifier 64999 for use in documentation.

--- middle

# Introduction

{{Section 12.3 of -coap}} describes the registration procedures for the "CoAP Content-Formats" IANA registry within the "Constrained RESTful Environments (CoRE) Parameters" registry group {{IANA.core-parameters}}.
(Note that the columns of this registry have been revised according to {{Err4954}}.)
In particular, it defines the rules for obtaining CoAP Content-Format identifiers from the "IETF Review or IESG Approval" range of the registry (256-9999) as well as from the First Come First Served (FCFS) range of the registry (10000-64999).
For the FCFS range, these rules do not involve the Designated Expert (DE) and are managed solely by IANA personnel to finalize the registration.

Unfortunately, the rules do not explicitly require checking that the combination of Content-Type (i.e., Media Type with optional parameters) and Content Coding associated with the requested CoAP Content-Format is semantically valid.
This task is generally non-trivial, requires knowledge from multiple documents and technologies, and should not be solely demanded from the registrar.
This lack of guidance may engender confusion in both the registering party and the registrar, and has already led to erroneous registrations.

In {{iana}}, this document updates {{-coap}} by modifying the registration procedures for the "CoAP Content-Formats" registry to mitigate the risk of unintentional or malicious errors.
These updates amend the different ranges of the registry, introduce a review procedure to be performed for most ranges of the registry, and allow the registration of temporary Content-Format identifiers.
This document also introduces a new column, "Media Type", to the registry.
Furthermore, this document reserves Content-Format identifier 64999 for use in documentation.

# Conventions and Definitions

This document uses the terms "Media Type", "Content Coding", "Content-Type", and "Content Format" as defined in {{Section 2 of -senml-ct}}.
In this document, those terms are fully capitalized.

# Security Considerations

This document hardens the registration procedures of CoAP Content-Formats in ways that reduce the chances of malicious manipulation of the associated registry.

Other than that, it does not change the Security Considerations of {{-coap}}.

# IANA Considerations

This document updates the IANA procedures defined in {{-coap}} for registering CoAP Content-Formats as described in {{iana}}.
It also removes a note that was added to the registry as a temporary patch ({{temp-note-removal}}), adds a new note concerning temporary registrations ({{new-note-add}}) and reserves Content-Format ID 64999 for documentation ({{reserve-64999}}).

## CoAP Content-Formats Registry {#iana}

This section and its subsections replace {{Section 12.3 of -coap}}.

[^replace-self]

Internet media types are identified by a string, such as "application/xml" {{?RFC2046}}.
In order to minimize the overhead of using Media Types to indicate the format of payloads, {{-coap}} has defined a registry for a subset of Internet Media Types to be used in CoAP and assigned each, in combination with a Content Coding, a numeric identifier.
The name of the registry is "CoAP Content-Formats", within the "CoRE Parameters" registry group.

Each entry in the registry must include the Media Type registered with IANA, the numeric identifier in the range 0-65535 to be used for that Media Type in CoAP, the Content Coding associated with this identifier, and a reference to a document describing what a payload with that Media Type means semantically.

CoAP does not include a separate way to convey Content Coding information with a request or response, and for that reason the Content Coding is also specified for each identifier (if any).
If multiple Content Codings  will be used with a Media Type, then a separate Content-Format identifier for each is to be registered.
Similarly, other parameters related to an Internet Media Type can be defined for a CoAP Content-Format entry.

The registration procedures for CoAP Content-Formats are described in {{tbl-new-cf-proc}}.

| Range | Registration Procedures | Notes |
|--------|--------|--------|
| 0-255 | Expert Review | Review procedure described in {{&SELF}}, {{checks}}. |
| 256-9999 | IETF Review with Expert Review or IESG Approval with Expert Review | Review procedure described in {{&SELF}}, {{checks}} |
| 10000-19999 | Expert Review | Review procedure described in {{&SELF}}, {{checks}}. |
| 20000-32999 | First Come First Served (FCFS) | FCFS is allowed if the registration: <br> * has no parameters, and <br> * has an empty Content Coding, and <br> * the Media Type is not yet used in this registry, and <br> * the Media Type is registered (or approved for registration) in the "Media Types" registry {{IANA.media-types}}. |
| 33000-64998 | Expert Review | Review procedure described in {{&SELF}}, {{checks}}. |
| 64999 | - | Reserved for Documentation |
| 65000-65535 | Experimental Use | No operational use |
{: #tbl-new-cf-proc title="CoAP Content-Formats: Registration Procedures"}

Because the namespace of single-byte identifiers is so small, the IANA policy for additions in the range 0-255 inclusive to the registry is "Expert Review" as described in {{Section 4.5 of -iana-cons}}.
For the handling of temporary allocations within the 0-255 range see also {{expert-review-7120-exemptions}}.

The 256-9999 range has registration procedures requiring "IETF Review with Expert Review" or "IESG Approval with Expert Review." In particular:

* All assignments according to "IETF Review with Expert Review" are made on an "IETF Review" basis per {{Section 4.8 of -iana-cons}} with "Expert Review" additionally required per {{Section 4.5 of -iana-cons}}.

  The procedure for early IANA allocation of "standards track code points" defined in {{-iana-early}} also applies. When such a procedure is used, IANA will ask the Designated Expert(s) to approve the early allocation before registration. In addition, working group chairs are encouraged to consult the Expert(s) early during the process outlined in {{Section 3.1 of -iana-early}}.

* All assignments according to "IESG Approval with Expert Review" are made on an "IESG Approval" basis per {{Section 4.10 of -iana-cons}} with "Expert Review" additionally required per {{Section 4.5 of -iana-cons}}.

The registration policy for the 10000-19999 and 33000-64998 ranges is "Expert Review", following the procedure described in {{checks}}.

The registration policy for the 20000-32999 range is FCFS.
A registration request for this range must consist solely of a registered Media Type name in the "Content Type" field, without any parameter names or "Content Coding", and the Media Type must not have been used in this registry yet.
If the criteria do not apply, a registration for a different range (which requires Expert Review) can be requested.

The identifiers between 65000 and 65535 inclusive are reserved for experiments.
They are not meant for vendor-specific use of any kind and MUST NOT be used in operational deployments.

In machine-to-machine applications, it is not expected that generic Internet media types such as text/plain, application/xml or application/octet-stream are useful for real applications in the long term.
It is recommended that M2M applications making use of CoAP request new Internet media types from IANA indicating semantic information about how to create or parse a payload.
For example, a Smart Energy application payload carried as CBOR might request a more specific type like application/se+cbor.

### Temporary Content-Format Registrations

This section clarifies that the "CoAP Content-Formats" registry allows temporary registrations within the 0-64998 range.

A temporary registration may be created for example by an IANA early allocation action {{-iana-early}}.
If the referenced Media Type is provisional (that is, included in the IANA "Provisional Standard Media Type" registry {{IANA.provisional-standard-media-types}}) then a created registration is always temporary.

A temporary registration is marked as such by IANA in the corresponding registry entry.
Once the required registration procedure (defined in {{tbl-new-cf-proc}}) for the temporary ID has successfully completed, and the referenced Media Type is included in the IANA Media Types registry {{IANA.media-types}}, IANA must remove any indication about the temporary nature of the registration so that the entry becomes permanent.

If a temporary registration does not successfully complete the registration procedure, IANA must remove the entry and set the Content-Format ID value back to "Unassigned".
This may happen for example when an Internet-Draft requesting a Content-Format ID is abandoned.
If a temporary registration (in any range) refers to a provisional Media Type that is abandoned, IANA must remove the entry and set the Content-Format ID value back to "Unassigned".

Note that in the 10000-64998 range the abandonment of a document requesting a Content-Format ID does not cause an entry to be removed.
That is because the required registration procedure for this range does not require completion of any standards process, nor does it require a registering document.

{:#expert-review-7120-exemptions}
Temporary registrations within the 0-255 range are exempt from the formal renewal process outlined in {{-iana-early}}.
Specifically, IANA will not monitor the removal of registrations in this range.
Instead, the Designated Experts direct IANA to carry out this task.

### Adding the Media Type Column to the Registry

To assist users of the "CoAP Content-Formats" registry in finding detailed information about the Media Type associated with each CoAP Content-Format, and to ensure that a Media Type exists before a new entry can be registered, IANA is requested to add a new column "Media Type" to the registry.
This new column is placed directly to the right of the existing "Content Type" column.

The "Media Type" field for each entry lists the (base) Media Type name and provides a hyperlink to registration information for that Media Type as recorded by IANA.
If the Media Type is provisional, the hyperlink points to the IANA "Provisional Standard Media Type" registry {{IANA.provisional-standard-media-types}}.
If a provisional Media Type becomes a permanent Media Type, IANA must update the "Media Type" field in the associated registry entries to ensure the hyperlink directs to the registration information for that Media Type.

Note that the registration request procedure remains unchanged. A requester does not need to fill out the "Media Type" field separately, as the necessary information is already provided in the "Content Type" field of the request.

### Expert Review Procedure {#checks}

The Designated Expert (DE) is instructed to perform the Expert Review, as described by the following checklist:

1. The combination of Content-Type and Content Coding for which the registration is requested must not be already present in the "CoAP Content-Formats" registry;
1. The Media Type associated with the requested Content-Format must either be registered in the "Media Types" registry {{IANA.media-types}} or approved for registration. Alternatively, it may be listed in the "Provisional Standard Media Type" registry {{IANA.provisional-standard-media-types}}. The use of provisional standard Media Types is only permitted for Content-Format identifiers within the ranges of 0-255 and 256-9999;
1. The optional parameter names must have been defined in association with the Media Type, and any parameter values associated with such parameter names must be as permitted;
1. The Content Type must be in the preferred format defined in {{preferred-format}};
1. If a Content Coding is specified, it must exist (or must have been approved for registration) in the "HTTP Content Coding" registry of the "Hypertext Transfer Protocol (HTTP) Parameters" {{IANA.http-parameters}}.

For the 0-255 range, in addition to the checks described above, the DE is instructed to also evaluate the requested codepoint concerning the limited availability of the 1-byte codepoint space.
For the 256-64998 range, a similar criterion may also apply where combinations of Media Type parameters and Content Coding choices consume considerable codepoint space.

### Examples for Invalid Registration Requests

This section provides examples of registration requests for the "CoAP Content-Formats" Registry that are invalid but would be approved under the procedure defined in {{Section 12.3 of -coap}}.
The checklist defined in {{checks}} should prevent any of these attempts from succeeding.
These examples serve as a representative, but not exhaustive, sample to train the DE's eye on invalid registration attempts.

All the example registration requests use a CoAP Content-Format with identifier 64999.

For each of the following example registration requests, one can create a similar instance where the requested registration is for a CoAP Content-Format identifier within the "IETF Review or IESG Approval" range.
Likewise, such registrations must not be allowed to succeed.

#### The Media Type is Unknown {#ex-unknown-mt}

The registrant requests an FCFS Content-Format ID for an unknown Media Type:

| Content Type | Content Coding | ID |
|--|--|--|
| application/unknown+cbor | - | 64999 |
{: align="left" title="Attempt at Registering Content-Format for an Unknown Media Type"}

#### The Media Type Parameter is Unknown

The registrant requests an FCFS Content-Format ID for an existing Media Type with an unknown parameter:

| Content Type | Content Coding | ID |
|--|--|--|
| application/cose;unknown-parameter=1 | - | 64999 |
{: align="left" title="Attempt at Registering Content-Format for Media Type with Unknown Parameter"}

#### The Media Type Parameter Value is Invalid

The registrant requests an FCFS Content-Format ID for an existing Media Type with an invalid parameter value:

| Content Type | Content Coding | ID |
|--|--|--|
| application/cose;cose-type=invalid | - | 64999 |
{: align="left" title="Attempt at Registering Content-Format for Media Type with Invalid Parameter Value"}

#### The Content Coding is Unknown

The registrant requests an FCFS Content-Format ID for an existing Media Type with an unknown Content Coding:

| Content Type | Content Coding | ID |
|--|--|--|
| application/senml+cbor | inflate | 64999 |
{: align="left" title="Attempt at Registering Content-Format with Unknown Content Coding"}

#### Duplicate Entry with Default Media Type Parameters

The registrant requests an FCFS Content-Format ID for a Media Type that includes a parameter set to its default value, while
a (hypothetical) Content-Format ID 64900 is already registered for this Media Type without that parameter.
As a result, this could lead to the creation of two separate Content-Format IDs for the same "logical" entry.

| Content Type | Content Coding | ID |
|--|--|--|
| application/my | - | 64900 |
| application/my;parameter=default | - | 64999 |
{: align="left" title="Attempt at Registering an Equivalent Logical Entry with a Different Content-Format ID (1)"}

#### Duplicate Entry with Default Content Coding

The registrant requests an FCFS Content-Format ID for the "identity" Content Coding, which is the default coding.
If accepted, this request would duplicate an entry with (hypothetical)
Content-Format ID 64900 where the "Content Coding" field is left empty.

| Content Type | Content Coding | ID |
|--|--|--|
| application/my | - | 64900 |
| application/my | identity | 64999 |
{: align="left" title="Attempt at Registering an Equivalent Logical Entry with a Different Content-Format ID (2)"}

#### Duplicate Entry with Equivalent Parameter

The registrant requests an FCFS Content-Format ID for a Media Type that includes a parameter.
The value of this parameter appears distinct from that of a (hypothetical) previously registered Content-Format ID 64900 that also includes this parameter.
However, the semantics of the parameter value are identical to the existing registration.

In this example, the `eat_profile` parameter value (which can be any URI) is set as a Uniform Resource Name (URN) {{?RFC8141}}.
Since for URNs, the Namespace Identifier (`example` in this example) is defined as case insensitive, the two registrations are semantically identical.

| Content Type | Content Coding | ID |
|--|--|--|
| application/eat+cwt;eat_profile="urn:example:1" | - | 64900 |
| application/eat+cwt;eat_profile="urn:EXAMPLE:1" | - | 64999 |
{: align="left" title="Attempt at Registering an Equivalent Logical Entry with a Different Content-Format ID (3)"}

### Preferred Format for the Content Type Field {#preferred-format}

This section defines the preferred string format for including a requested Content Type into the "CoAP Content-Formats" registry.
During the review process, the Designated Expert(s) or IANA may rewrite a requested Content Type into this preferred string format before approval.

The preferred string format is as defined in {{Section 8.3.1 of -http-sema}} and follows these rules:

1. For any case-insensitive elements, lowercase characters are used.
1. Parameter values are only quoted if the value is such that it requires use of `quoted-string` per {{Section 5.6.6 of -http-sema}}.
   Otherwise, a parameter value is included unquoted.
1. A single semicolon character without any adjacent whitespace characters is used as the separator between Media Type and parameters.

## Temporary Note Removal {#temp-note-removal}
{:removeinrfc}

The following note has been added to the registry as a temporary fix:

> "Note: The validity of the combination of Content Coding, Content Type and parameters is checked prior to assignment."

IANA is instructed to remove this note from the registry when this document is approved for publication.
RFC-Editor: please remove this section once the note has been removed.

## New Note Addition {#new-note-add}

[^replace-self]

IANA is instructed to add the following note to the registry:

> "Note: As per {{&SELF}}, temporary registrations within the 0-255 range are approved by Designated Experts.
> These registrations are not subject to the formal {{-iana-early}} renewal process."

## Reserving Content-Format Identifier 64999 for Documentation {#reserve-64999}

IANA is instructed to reserve Content-Format identifier 64999 for use in documentation.

--- back

# Acknowledgments
{:numbered="false"}

Thank you
Amanda Baber,
Carsten Bormann,
Christer Holmberg,
Francesca Palombini,
Marco Tiloca,
and
Rich Salz
for your reviews, comments, suggestions, and fixes.

[^replace-self]: RFC Editor: in this section, please replace {{&SELF}} with the RFC number assigned to this document and remove this note.
