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

__1. Is one who they claim to be?__

__2. Is one to be trusted for what they claim to be trusted for?__

To address these needs, this specification codifies a
[[ref:Trust List]] data format that can be used to
articulate trust assertions and relationships.

This specification does not define transport protocols, specific endpoints, or other means for conveying the formatted objects it codifies, but encourages other specifications and projects that do define such mechanisms to utilize these data formats within their flows.

## Status of This Document

The Trust List is a _STRAWMAN_ specification under development within the Decentralized Identity Foundation (DIF). It incorporates requirements and learnings from related work of many active industry players into a shared specification that meets the collective needs of the community.

This specification is regularly updated to reflect relevant changes, and we encourage active engagement on [GitHub](https://github.com/decentralized-identity/trust-establishment/issues) and other mediums (e.g., [DIF Slack](https://difdn.slack.com/archives/C4X50SNUX)).

## Terminology

[[def:Claim, Claims]]
~ An assertion made about a [[ref:Subject]]. Used as an umbrella term for
Credential, Assertion, Attestation, etc.

[[def: Verifiable Credential, VC, VCs]]
~ Refers to the [W3C specification](https://www.w3.org/TR/vc-data-model) of the Verifiable Credentials Data Model.

[[def:Trust Establishment]]
~ The process by which one [[ref:Party]] accepts as necessary to begin [[ref:Verifiable Interactions]] with another [[ref:Party]]   

[[def:Trust List, Trust Lists]] 
~ A document used to communicate the who [[ref:Parties]] views as trusted for specific [[ref:Claims]].

[[def:Endorsement]]
~ An object representing a digital signature over a Trust List that communicates approval and support of a Trust List's content.

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

[[def:Credential Type, Credential Types]]
~ A [[ref:Verifiable Credential]] always has a [_type_ property](https://www.w3.org/TR/vc-data-model/#types) referencing one or more URIs
which provide information semantic meaning and classification for the data in a credential.

## Structure of this Document
This document has two primary sections: In the first, there is a model for defining the set of information a [[ref:Verifier]] would like to have in case of [[ref:Trust Establishment]], and in the second, there are suggested models to facilitate [[ref:Trust Establishment]] and utilization of [[ref:Trust Lists]] in practice.

Examples in this document use the [Verifiable Credentials Data Model](https://www.w3.org/TR/vc-data-model/) and the [Decentralized  Identifiers (DIDs)](https://www.w3.org/TR/did-core/) formats for illustrative purposes only; this specification is intended to support any JSON-serializable [[ref:Claim]] format.

## Trust List

[[ref:Trust Lists]] are objects that articulate information a [[ref:Verifier]] requires in order to facilitate a [[ref: Verifiable Interaction]] between [[ref:Parties]]. Trust Lists aid [[ref:Verifiers]] in deciding how or whether to interact with a [[ref:Holder]], given the [[ref:Claims]] they [[ref:Present]]. [[ref:Trust Lists]] are composed of `trusted_entities`, which describe [[ref:Parties]] that they accept for specific [[ref:Claims]] based on [[ref:Schemas]], which are defined in the `trusted_for` property of the object.

::: note

A [[ref:Trust List]] can exist on its own, or with [Endorsements](#endorsements). A Trust List by itself is referred to as a _Trust List_, and one with endorsements is referred to as an _Endorsed Trust List_.

:::

::: example Endorsed Trust List - Full Example

```json
{
    "trust_list": {
        "id": "32f54163-7166-48f1-93d8-ff217bdb0653",
        "created": "2010-01-01T19:23:24Z",
        "version": 3,
        "description": "Trust List for Name and Date of Birth claims",
        "authors": [
          {
            "id": "did:test:abcd",
            "name": "Issuer ABCD",
            "description": "Corp ABCD who has a website at https://abcd.website",
            "trust_reference": "https://abcd.website/.well-known/did-configuration.json"
          }
        ],
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
:::

### General Composition

The following properties are for use at the top-level of a [[ref:Trust List]]. Any properties that are not defined below ****MUST**** be ignored.

- `id` - The object ****MUST**** contain an `id` property. The value of this property ****MUST**** be a string. The string ****SHOULD**** provide a unique ID for the desired context. For example, a [UUID](https://tools.ietf.org/html/rfc4122) such as `32f54163-7166-48f1-93d8-ff217bdb0653` could provide an ID that is unique in a global context, while a simple string such as `my_trust_list-1` could be suitably unique in a local context.
- `created` - The object ****MUST**** contain a `created` property proving a date-time value for when the object was created. The value of this property ****MUST**** be a [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339) compliant timestamp value.
- `version` - The object ****MUST**** contain a `version` property. The value of this property ****MUST**** be a number. It is recommended that the value is a monotonic increasing integer value.
- `authors` - The object ****MUST**** contain an `authors` property. Its value ****MUST**** be an array of author objects, accurately identifying those responsible for creating the trust list. The object ****MUST**** be composed as follows:
    - The `authors` object ****MUST**** contain an `id` property. The value of this property ****MUST**** be a string value representing the [[ref:DID]] of the author.
    - The `authors` object ****MAY**** contain a `name` property. The value of this property is expected to be a human-readable name for the author.
    - The `authors` object ****MAY**** contain a `description` property. The value of this property is expected to be a human-readable description of the author.
    - The `authors` object ****MAY**** contain a `trust_reference` property. The value of this property is expected to be a method by which the list author has established trust in their [[ref:DID]].
- `trusted_for` – The object ****MUST**** contain a `trusted_for` property. The object contains information denoting which [[ref:Schemas]] and [[ref:Credential Types]] a specific [[ref:Issuer]] is to be trusted for issuing against, and ****MUST**** be composed as follows:
    - The `trusted_for` object ****MAY**** contain a `name` property. If present its value ****MUST**** be a human-friendly string that identifies the `trusted_for` element.
    - The `trusted_for` object ****MAY**** contain a `description` property. If present its value ****MUST**** be a string that describes what the schema is in greater detail.
    - The `trusted_for` object ****MUST**** contain a `schemas`, and its value ****MUST**** be an array of string URI values to [[ref:Schema]] or [[ref:Credential Type]] resources.
- `trusted_entities` - The object ****MUST**** contain a `trusted_entities` property. Its value describes the entities, or [[ref:Parties]] which are to be trusted for the items in the `trusted_for` property. The object ****MUST**** be composed as follows:
  - The `trusted_entities` object ****MUST**** contain an `identifiers` property. The property ****MUST**** be an array of strings with one or more elements uniquely identifying the [[ref:Party]] via a [[ref:DID]].
  - The `trusted_entities` object ****MAY**** contain a `name` property. If present its value ****MUST**** be a human-friendly string that identifies the trusted entity.
  - The `trusted_entities` object ****MAY**** contain a `description` property. If present its value ****MUST**** be a string that describes what the trusted entity is in greater detail.

**Example JSON Object**

```json
{
    "trust_list": {
        "id": "32f54163-7166-48f1-93d8-ff217bdb0653",
        "created": "2010-01-01T19:23:24Z",
        "version": 3,
        "description": "Trust List for Name and Date of Birth claims",
        "authors": [
          {
            "id": "did:test:abcd",
            "name": "Issuer ABCD",
            "description": "Corp ABCD who has a website at https://abcd.website",
            "trust_reference": "https://abcd.website/.well-known/did-configuration.json"
          }
        ],
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
    }
}
```


### Endorsements

A Trust List may optionally contain an `endorsements` property. If present, the Trust List should be referred to as an _Endorsed Trust List_. Endorsements allow [[ref:DID]]-identified [[ref:Parties]] to assert that they support a given Trust List. Practically, this is a means by which an endorser can communicate which [[ref:Claims]] they accept from which [[ref:Issuers]]. Anyone can endorse a [[ref:Trust List]]. The storage, propagation, and endorsement process for [[ref:Trust Lists]] is out of the scope of this specification.

The following properties are for use at the top-level of an [[ref:Endorsement]], which is represented as an array of objects. Any properties that are not defined below MUST be ignored. The objects that comprise the Endorsement array are defined as follows:
  - `endorser` – The object ****MUST**** contain an `endorser` property, a string value identifying the endorsing [[ref:Party]] via a [[ref:Decentralized Identifier]].
  - `endorsed` - The object ****MUST**** contain an `endorsed` property. The value of this property ****MUST**** be a [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339) compliant timestamp value, denoting the point in time the endorsement was created.
  - `proof` – The object ****MUST**** contain an embedded [Linked Data Proof](https://w3c-ccg.github.io/data-integrity-spec/) object.

::: todo Proofs
  Find a way to nicely support JWTs / other proof formats.
:::

```json
    "endorsements": [
        {
            "endorser": "did:abcd:1234",
            "endorsed": "2012-01-01T19:23:24Z",
            "proof": { ... }
        },
        {
            "endorser": "did:abcd:1234",
            "endorsed": "2012-01-01T19:23:24Z",
            "proof": { ... }
        }
    ]
```

## Appendix

### JSON Schema

::: todo JSON Schema
  Add JSON Schemas for all objects once the spec is a bit more solid.
:::

### Examples

::: todo Examples
  Add examples of different configurations, multiple proof formats.
:::

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

[[def: Linked Data Proofs, Data Integrity]]
  - [Data Integrity 1.0](https://w3c-ccg.github.io/data-integrity-spec/). Dave Longley, Manu Sporny. 30 April 2022. Status: W3C Draft Community Report.
