Trusted Issuer Establishment 0.0.1
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
[[ref:Trusted Issuer Establishment]] data format that can be used by [[ref:Issuers] to
articulate trust relations, and assertions for those relations.

This specification does not define transport, protocols, specific endpoints, or other means for conveying the formatted objects it codifies, but encourages other specifications and projects that do define such mechanisms to utilize these data formats within their flows.

## Status of This Document

Trusted Issuer Establishment is a _STRAWMAN_ specification under development within the Decentralized Identity Foundation (DIF). It incorporates requirements and learnings from related work of many active industry players into a shared specification that meets the collective needs of the community.

This specification is regularly updated to reflect relevant changes, and we encourage active engagement on [GitHub](https://github.com/decentralized-identity/trust-establishment/issues) and other mediums (e.g., [DIF Slack](https://difdn.slack.com/archives/C4X50SNUX)).

## Terminology

[[def:Claim, Claims]]
~ An assertion made about a [[ref:Subject]]. Used as an umbrella term for
Credential, Assertion, Attestation, etc.

[[def: Verifiable Credential, VC, VCs]]
~ Refers to the [W3C specification](https://www.w3.org/TR/vc-data-model) of the Verifiable Credentials Data Model.

[[def:Trust Establishment]]
~ The process by which one [[ref:Party]] accepts as necessary to begin [[ref:Verifiable Interactions]] with another [[ref:Party]]   

[[def:Trusted Issuer Establishment, Trusted Issuer Establishment Documents, TIE, TIE Document, TIE Documents]] 
~ A document used to communicate the who [[ref:Parties]] views as trusted for specific [[ref:Claims]].

[[def:Endorsement, Endorse, Endorsements]]
~ An object representing a digital signature over a Trust List that communicates approval and support of a Trust List's content.

[[def:Identified Parties, Party, Parties]] 
~ Entities that establish their identity by means of a [[def:Decentralized Identifier]]. Parties are commonly referred to as [[ref:Issuers]], [[ref:Holders]], and [[ref:Verifiers]].

[[def:Trust Host, Host]]
~ Entities that are responsible for hosting and communicating [[ref: TIE Documents]].

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
This document has two primary sections: In the first, there is a model for defining the set of information a [[ref:Verifier]] would like to have in case of [[ref:Trusted Issuer Establishment]] ([[ref:TIE]]), and in the second, there are suggested models to facilitate [[ref:Trusted Issuer Establishment]] and utilization of [[ref:TIE Document]]s in practice.

Examples in this document use the [Verifiable Credentials Data Model](https://www.w3.org/TR/vc-data-model/) and the [Decentralized  Identifiers (DIDs)](https://www.w3.org/TR/did-core/) formats for illustrative purposes only; this specification is intended to support any JSON-serializable [[ref:Claim]] format.

## Trusted Issuer Establishment

[[ref:Trusted Issuer Establishment Documents]] are objects that articulate information a [[ref:Verifier]] requires in order to facilitate a [[ref: Verifiable Interaction]] between [[ref:Parties]]. Most often, [[ref:TIE Documents]] are published by a [[ref:Verifier]] describing which [[ref:Issuers]] they trust for what [[ref:Claims]]. [[ref:TIE Documents]] also aid [[ref:Verifiers]] in determining whether to interact with a [[ref:Holder]], given the [[ref:Claims]] they [[ref:Present]]. [[ref:TIE Documents]] are composed of `entries` objects, which describe [[ref:Parties]] that they allow for specific [[ref:Claims]] based on [[ref:Schemas]], which are defined in the `credentials` property of the object.

::: note

A [[ref:TIE Document]] can exist on its own, or with [Endorsements](#endorsements). A TIE Document by itself is referred to as a _Trusted Issuer Establishment Document_ or _TIE Document_. A _TIE Document_ with endorsements is referred to as an _Endorsed TIE Document_.

:::

::: example Trusted Issuer Establishment Document - Full Example

```json
{
    "id": "32f54163-7166-48f1-93d8-ff217bdb0653",
    "created": "2010-01-01T19:23:24Z",
    "version": 3,
    "description": "Trusted Issuers for Dank Memes",
    "author": "did:example:memeking",
    "entries": {
      "did:example:memester": [
          "example.com/dank-meme.schema.json",
          "example.com/danker-meme.schema.json"
      ],
      "did:example:illumemenati": [
          "example.com/supremeley-dank-meme.schema.json",
      ]
    },
    "endorsements": [
        {
            "endorser": "did:example:memeapprover",
            "endorsed": "2012-01-01T19:23:24Z",
            "proof": { ... }
        },
        {
            "endorser": "did:example:okboomer",
            "endorsed": "2012-01-01T19:23:24Z",
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
- `author` - The ****MUST**** contain an `id` property. The value of this property ****MUST**** be a string value representing the [[ref:DID]] of the author.

::: note

A [[ref:TIE Document]] may be gain utility in being authored by multiple parties. In such cases, it is recommended to create a new [[ref:DID]] identity that represents multiple parties under a shared identity. Alternatively, one party can author, and others can provide [[ref:Endorsements]].

:::

- `entries` – The object ****MUST**** contain a `entries` property. The object contains information denoting which [[ref:Schemas]] and [[ref:Credential Types]] a specific [[ref:Issuer]] is to be trusted for issuing against, and ****MUST**** be composed as a map as follows. 
    - The `entries` object ****MUST**** have map keys as _string_ [[ref:DIDs]]
    - The `entries` object ****MUST**** have map values as _string arrays_, which ****MUST**** be an array of string URI values to [[ref:Schema]] or [[ref:Credential Type]] resources.

**Example JSON Object**

```json
{
    "id": "32f54163-7166-48f1-93d8-ff217bdb0653",
    "created": "2010-01-01T19:23:24Z",
    "version": 3,
    "description": "Trusted Issuers for Univeristy Degrees",
    "author": "did:example:employer",
    "entries": {
      "did:example:university-1": [
          "universities.org/bachelors-of-art-degree.schema.json",
          "universities.org/bachelors-of-science-degree.schema.json"
      ],
      "did:example:university-2": [
          "universities.org/bachelors-of-science-degree.schema.json",
          "universities.org/masters-degree.schema.json"
      ]
    }
}
```


### Endorsements

A [[ref: TIE Document]] may optionally contain an `endorsements` property. If present, the document should be referred to as an _Endorsed TIE Document__. Endorsements allow [[ref:DID]]-identified [[ref:Parties]] to assert that they support a given [[ref: TIE Document]]. Practically, this is a means by which an endorser can communicate which [[ref:Claims]] they accept from which [[ref:Issuers]]. Anyone can endorse a [[ref:TIE Document]]. The storage, propagation, and endorsement process for [[ref:TIE Documents]] is out of the scope of this specification.

The following properties are for use at the top-level of an [[ref:Endorsement]], which is represented as an array of objects. Any properties that are not defined below MUST be ignored. The objects that comprise the Endorsement array are defined as follows:
  - `endorser` – The object ****MUST**** contain an `endorser` property, a string value identifying the endorsing [[ref:Party]] via a [[ref:Decentralized Identifier]].
  - `endorsed` - The object ****MUST**** contain an `endorsed` property. The value of this property ****MUST**** be a [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339) compliant timestamp value, denoting the point in time the endorsement was created.
  - `proof` – The object ****MUST**** contain an embedded [Linked Data Proof](https://w3c-ccg.github.io/data-integrity-spec/) object.

::: todo Proofs
  Find a way to nicely support JWTs / other proof formats.
:::

```json
{   
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
}    
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
