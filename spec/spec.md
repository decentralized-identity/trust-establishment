Trust Establishment 0.0.1
==================

**Specification Status:** Strawman

**Latest Draft:**
  [identity.foundation/trust-establishment](https://identity.foundation/trust-establishment)

<!-- -->
**Editors:**
~ [Gabe Cohen](https://www.linkedin.com/in/cohengabe/) (Block)
~ [Daniel Buchner](https://www.linkedin.com/in/dbuchner/) (Block)
~ [Mike Ebert](https://www.linkedin.com/in/michaelebert/) (Indicio)
~ [Juan Caballero](https://www.linkedin.com/in/juan-caballero/) (Centre Consortium)

**Contributors:**

**Participate:**
~ [GitHub repo](https://github.com/decentralized-identity/trust-establishment)
~ [File a bug](https://github.com/decentralized-identity/trust-establishment/issues)
~ [Commit history](https://github.com/decentralized-identity/trust-establishment/commits/master)

------------------------------------

## Abstract

Trust in the decentralized identity space is a problem that many have tried to solve. This specification aims to take a piece of the problem around [[ref:Trust Establishment]]: communicating trust between [[ref:parties]]. Decentralized [[ref:Trust Establishment]] is the means by which [[ref:parties]] answer two key questions about one another:

1. Is one who one claims to be?
2. Is one to be trusted for what one claims to be trusted for?

To address these needs, this specification codifies a
[[ref:Trust List]] data format that can be used to
articulate trust assertions and relationships.

This specification does not define transport protocols, specific endpoints, or
other means for conveying the formatted objects it codifies, but encourages
other specifications and projects that do define such mechanisms to utilize
these data formats within their flows.

## Status of This Document

The Trust List is a _STRAWMAN_ specification under development within
the Decentralized Identity Foundation (DIF). It incorporates requirements and
learnings from related work of many active industry players into a shared
specification that meets the collective needs of the community.

This specification is regularly updated to reflect relevant changes, and we
encourage active engagement on
[GitHub](https://github.com/decentralized-identity/trust-establishment/issues)
and other mediums (e.g., [DIF Slack](https://difdn.slack.com/archives/C4X50SNUX)).

## Terminology

[[def:Claim, Claims]]
~ An assertion made about a [[ref:Subject]]. Used as an umbrella term for
Credential, Assertion, Attestation, etc.

[[def:Trust Establishment]]
~ The process by which one [[ref:Party]] accepts as necessary to begin [[ref:Verifiable Interactions]] with another [[ref:Party]]   

[[def:Trust List, Trust Lists]] 
~ A document used to communicate the who [[ref:Parties]] views as trusted for specific [[ref:Claims]].

[[def:Identified Parties, Party, Parties]] 
~ Entities that establish their identity by means of a [[def:Decentralized Identifier]]. Parties are commonly referred to as [[ref:Issuers]], [[ref:Holders]], and [[ref:Verifiers]].

[[def:Trust Host, Host]]
~ Entities that are responsible for hosting and communicating [[Trust Lists]].

[[def:Issuer, Issuers]]
~ Issuers are entities that issue [[ref:Claims]] to [[ref:Holders]], and may be represented as an [[ref:Identified Party]] inside of a [[ref:Trust List]].

[[def:Holder, Holders]]
~ Holders are entities that submit [[ref:Claims]] to [[ref:Verifiers]] which contain [[ref:Parties]] that may be identified in a [[ref:Trust List]] to satisfy a [[ref:Presentation]] interaction.

[[def:Verifier, Verifiers]]
~ Verifiers are entities that define what [[Parties]] they accept a
[[ref:Credential]] from in order to proceed with an
an interaction.

[[def:Presentation, Present]]
~ Presentations are a type of verifiable interaction in which one [[ref:Party]] submits a [[ref:Claim]] or set of [[ref:Claims]] to a [[ref:Verifier]].

[[def:Decentralized Identifier, Decentralized Identifier, DID]]
~ A [W3C specification](https://www.w3.org/TR/did-core/) describing an _identifier that enables verifiable, decentralized digital identity_.

[[def:Verifiable Interaction, Verifiable Interactions]]
~ An interaction (e.g. request a claim, present a claim, establish trust) which happens between [[ref:Parties]] using [[ref:Decentralized Identifiers]].

[[def:Schema, Schemas]]
~ A schema is a vocabulary for a [[ref:Claim]]. Commonly used schema formats include [[ref:JSON Schema]] or [[ref:JSON Linked Data]] as surfaced through [schema.org](https://schema.org/).

## Structure of this Document
This document has two primary sections: In the first, there is a model for defining the set of information a [[ref:Verifier]] would like to have in case of [[ref:Trust Establishment]], and in the second, there are suggested models to facilitate [[ref:Trust Establishment]] and utilization of [[ref:Trust Lists]] in practice.

Examples in this document use the [Verifiable Credentials Data Model](https://www.w3.org/TR/vc-data-model/) and the [Decentralized  Identifiers (DIDs)](https://www.w3.org/TR/did-core/) formats for illustrative purposes only; this specification is intended to support any JSON-serializable [[ref:Claim]] format.

## Trust List

[[ref:Trust Lists]] are objects that articulate information a
[[ref:Verifier]] requires in order to facilitate a [[ref: Verifiable Interaction]] between [[ref:Parties]]. These help the [[ref:Verifier]] to decide how or
whether to interact with a [[ref:Holder]], given the [[ref:Claims]] they [[ref:Present]]. [[ref:Trust Lists]] are
composed of `trusted_entities`, which describe [[ref:Parties]] that they accept for specific [[ref:Claims]] based on [[ref:Schemas]].

::: example Trust List - Minimal Example
```json
```
:::


The following properties are for use at the top-level of a
[[ref:Presentation Definition]]. Any properties that are not defined below MUST
be ignored, unless otherwise specified by a [[ref:Feature]];

- `id` - The [[ref:Presentation Definition]] ****MUST**** contain an `id`
  property. The value of this property ****MUST**** be a string. The string
  ****SHOULD**** provide a unique ID for the desired context. For example, a
  [UUID](https://tools.ietf.org/html/rfc4122) such as `32f54163-7166-48f1-93d8-f
  f217bdb0653` could provide an ID that is unique in a global context, while a
  simple string such as `my_presentation_definition_1` could be suitably unique
  in a local context.
- `input_descriptors` - The [[ref:Presentation Definition]]  ****MUST****
  contain an `input_descriptors` property. Its value ****MUST**** be an array of
  [[ref:Input Descriptor Objects]], the composition of which are described in
  the [`Input Descriptors`](#input-descriptors) section below.

  All inputs listed in the `input_descriptors` array are required for submission,
  unless otherwise specified by a [[ref:Feature]].

- `name` - The [[ref:Presentation Definition]] ****MAY**** contain a `name`
  property. If present, its value ****SHOULD**** be a human-friendly string
  intended to constitute a distinctive designation of the
  [[ref:Presentation Definition]].

**Example JSON Object**

```json
{
    "trust_list": {
        "id": "32f54163-7166-48f1-93d8-ff217bdb0653",
        "author": "did:test:abcd",
        "created": "2010-01-01T19:23:24Z",
        "version": "3",
        "trusted_for": [
          {
            "name": "Person's Name",
            "description": "A person's name",
            "schemas": ["schema.org/name", "example.com/name-schema.json"]
          },
          {
            "name": "Date of Birth",
            "description": "A person's date of birth",
            "schemas": ["schema.org/dob"]
          }
        ],
        "trusted_entities": [
            {
                "identifiers": ["did:test:1234"],
                "name": "Identifiers for Corporation A.",
                "description": "Corporation A is a universally recognized authority on names and date of birth"
            },
            {
                "identifiers": ["did:test:5678"],
                "name": "Identifiers for Corporation B.",
                "description": "Corporation B is a universally recognized authority on human credentials"
            },
        ]
    },
    "endorsements": [
        {
            "endorser": "did:abcd:1234",
            "endorsedOn": "2012-01-01T19:23:24Z",
            "proof": { ... }
        },
        {
            "endorser": "did:abcd:1234",
            "endorsedOn": "2012-01-01T19:23:24Z",
            "proof": { ... }
        }
    ]
}
```

## References

[[def: Verifiable Credentials, Verifiable Credential, VC, VCs]]
  - [Verifiable Credentials Data Model v1.1](https://www.w3.org/TR/vc-data-model/). Gregg Kellogg, Pierre-Antoine Champin, Manu Sporny, Grant Noble, Dave Longley, Daniel C. Burnett, Brent Zundel, Kyle Den Hartog. 03 March 2022. Status: W3C Recommendation.

[[def:Decentralized Identifiers, DIDs]]
  - [Decentralized Identifiers (DIDs) v1.0](https://www.w3.org/TR/did-core/). Manu Sporny, Amy Guy, Markus Sabadello, Drummond Reed, Dave Longley, Orie Steele, Christopher Allen. 03 August 2021. Status: W3C Proposed Recommendation.

[[def: JSON Schema]]
  - [JSON Schema](https://json-schema.org/)
  - [JSON Schema: A Media Type for Describing JSON Documents](https://json-schema.org/draft/2020-12/json-schema-core.html). A. Wright, H. Andrews, B. Hutton, G. Dennis. Status: 28 January 2020. Internet-Draft.

[[def: JSON-LD, JSON Linked Data]]
  - [JSON-LD](https://json-ld.org/)
  - [JSON-LD 1.1, _A JSON-based Serialization for Linked Data_](https://www.w3.org/TR/json-ld/). Manu Sporny, Dave Longley, Gregg Kellogg, Markus Lanthaler, Pierra-Antoine Champin, Niklas Lindström. 16 July 2020. Status: W3C Recommendation.

[[spec]]

## Appendix

### Examples
