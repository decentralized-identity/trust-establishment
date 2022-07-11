Sentiment Declaration 0.0.1
==================

**Specification Status:** Strawman

**Latest Draft:**
  [identity.foundation/trust-establishment/sentinemtn-declaration](https://identity.foundation/trust-establishment/sentiment-declaration)

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

Trust in the decentralized identity space is a problem that many have tried to solve. This specification aims to take a piece of the problem around [[ref:Semantic Declaration]]: a means by which an [[ref:Entity]] communicates their sentiment for a [[ref:Topic]] about a set of [[ref:Parties]]. [[ref: Sentiment Declaration]] is intended to be informational, and presciptions of any resulting actions taken based on the contents of the document are out of scope of this specification.

## Status of This Document

Sentiment Declaration is a _STRAWMAN_ specification under development within the Decentralized Identity Foundation (DIF). It incorporates requirements and learnings from related work of many active industry players into a shared specification that meets the collective needs of the community.

This specification is regularly updated to reflect relevant changes, and we encourage active engagement on [GitHub](https://github.com/decentralized-identity/trust-establishment/issues) and other mediums (e.g., [DIF Slack](https://difdn.slack.com/archives/C4X50SNUX)).

## Terminology

[[def:Claim, Claims]]
~ An assertion made about a [[ref:Subject]]. Used as an umbrella term for
Credential, Assertion, Attestation, etc.

[[def: Verifiable Credential, VC, VCs]]
~ Refers to the [W3C specification](https://www.w3.org/TR/vc-data-model) of the Verifiable Credentials Data Model.

[[def:Topic, Sentiment Topic]]
~ A [[ref:Schema]]-driven document that gives purpose to a sentiment document. A topic can be anything: from the tangibly measurable (e.g. success rate of a bussiness prossess) to the intangible (e.g. how one party feels about another).

[[def:Sentiment Declaration, Sentiment Declaration Document, Sentiment Declaration Documents, Sentiment Declarations]]
~ The process by which one [[ref:Party]] declares their sentiment about a given [[ref:Party]] for a given [[ref:Topic]].

[[def:Identified Parties, Party, Parties]] 
~ Entities that establish their identity by means of a [[def:Decentralized Identifier]]. Parties are commonly referred to as [[ref:Issuers]], [[ref:Holders]], and [[ref:Verifiers]].

[[def:Sentiment Host, Host]]
~ Entities that are responsible for hosting and communicating [[ref:Sentiment Declarations]].

[[def:Issuer, Issuers]]
~ Issuers are entities that issue [[ref:Claims]] to [[ref:Holders]], and may be represented as an [[ref:Identified Party]] inside of a [[ref:Sentiment Declaration]].

[[def:Holder, Holders]]
~ Holders are entities that submit [[ref:Claims]] to [[ref:Verifiers]] which contain [[ref:Parties]] that may be identified in a [[ref:Sentiment Declaration]] to satisfy a [[ref:Presentation]] interaction.

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
This document has two primary sections: In the first, there is a model for defining the set of information needed to create a [[ref:Topic]]. The second covers the information needed for a [[ref:Party]] to create a [[ref:Sentiment Declaration]] document.

Examples in this document use the [Verifiable Credentials Data Model](https://www.w3.org/TR/vc-data-model/) and the [Decentralized  Identifiers (DIDs)](https://www.w3.org/TR/did-core/) formats for illustrative purposes only; this specification is intended to support any JSON-serializable [[ref:Claim]] format.

## Sentiment Declaration

[[ref:Sentiment Declaration Documents]] are objects that articulate information a [[ref:Party]] publishes in order to share their _sentiment_ about a set of [[ref:DID]]-identified entities. [[ref:Issuers]], [[ref:Verifiers]], and [[ref:Holders]] ma find utility in understanding a [[ref:Party]]'s sentiment in making decisions in decentralized web interactions.

**Sentiment Topic Example**

::: example Sentiment Topic - Full Example

```json
{
  "$id": "https://example.com/trusted-supplier.schema.json",
  "$schema": "https://json-schema.org/draft/2019-09/schema",
  "title": "Supplier Trust",
  "type": "object",
  "properties": {
    "on_time_percentage": {
      "type": "number",
      "min": 0,
      "max": 100
    },
    "goods": {
      "type": "array",
      "items": {
          "format": "string"
      }
    }
  },
  "additionalProperties": false
}
```
:::

**Sentiment Declaration Example**


::: example Sentiment Declaration - Full Example

```json
{
  "id": "32f54163-7166-48f1-93d8-ff217bdb0653",
  "author": "did:example:alice",
  "created": "2010-01-01T19:23:24Z",
  "version": 2,
  "topic": "https://example.com/trusted-supplier.schema.json",
  "entries": {
    "did:example:bob": {
      "on_time_percentage": 92,
      "goods": ["applewood", "hotel buffet style", "thick cut"]
    },
    "did:example:carol": {
      "on_time_percentage": 74,
      "goods": ["oinkys", "porkys", "wilburs"]
    }
  }
}
```
:::

### Topic Composition

::: todo text on topics
what's a sentiment topic?
:::


A topic ****MUST**** be a [[ref:JSON Schema]] document that can be applied to any number of [[ref:Parties]] identified in the `entries` property of a [[ref:Sentiment Document]].


**Example JSON Object**

```json
{
  "$id": "https://example.com/trusted-supplier.schema.json",
  "$schema": "https://json-schema.org/draft/2019-09/schema",
  "title": "Supplier Trust",
  "type": "object",
  "properties": {
    "on_time_percentage": {
      "type": "number",
      "min": 0,
      "max": 100
    },
    "goods": {
      "type": "array",
      "items": {
        "format": "string"
      }
    }
  },
  "additionalProperties": false
}
```

### Sentiment Declaration Composition

::: todo text on sentiment declarations
what's a sentiment declaration?
:::

The following properties are for use at the top-level of a [[ref:Sentiment Declaration Document]]. Any properties that are not defined below ****MUST**** be ignored.

- `id` - The object ****MUST**** contain an `id` property. The value of this property ****MUST**** be a string. The string ****SHOULD**** provide a unique ID for the desired context. For example, a [UUID](https://tools.ietf.org/html/rfc4122) such as `32f54163-7166-48f1-93d8-ff217bdb0653` could provide an ID that is unique in a global context, while a simple string such as `my_sentiment_declaration-1` could be suitably unique in a local context.
- `author` - The ****MUST**** contain an `id` property. The value of this property ****MUST**** be a string value representing the [[ref:DID]] of the author.
- `created` - The object ****MUST**** contain a `created` property proving a date-time value for when the object was created. The value of this property ****MUST**** be a [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339) compliant timestamp value.
- `version` - The object ****MUST**** contain a `version` property. The value of this property ****MUST**** be a number. It is recommended that the value is a monotonic increasing integer value.
- `topic` - The object ****MUST**** contain `topic` property. The value of this property ****MUST**** be a URI identifying the [[ref:Topic]] of the [[ref:Sentiment Declaration]].
- `entries` – The object ****MUST**** contain a `entries` property which is a JSON map. and ****MUST**** be composed as a map as follows. 
    - The `entries` object ****MUST**** have map keys as _string_ [ref:DID]]s which identify [[ref:Parties]] for which sentiment is being expressed. 
    - The `entries` object ****MUST**** have map values as JSON objects conforming to the [[ref:Schema]] listed in the `topic` property.

::: todo signing & verification
how is the integrity of sentiment declarations maintained?
:::

```json
{
  "id": "32f54163-7166-48f1-93d8-ff217bdb0653",
  "author": "did:example:alice",
  "created": "2010-01-01T19:23:24Z",
  "version": "0.0.3",
  "topic": "https://example.com/trusted-supplier.schema.json",
  "entries": {
    "did:example:bob": {
      "on_time_percentage": 92,
      "goods": ["applewood", "hotel buffet style", "thick cut"]
    },
    "did:example:carol": {
      "on_time_percentage": 74,
      "goods": ["oinkys", "porkys", "wilburs"]
    }
  }
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
