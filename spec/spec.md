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
~ [Sam Curren](https://www.linkedin.com/in/samcurren/) (Indicio)
~ [Juan Caballero](https://www.linkedin.com/in/juan-caballero/) (Centre Consortium)

**Contributors:**

**Participate:**
~ [GitHub repo](https://github.com/decentralized-identity/trust-establishment)
~ [File a bug](https://github.com/decentralized-identity/trust-establishment/issues)
~ [Commit history](https://github.com/decentralized-identity/trust-establishment/commits/master)

------------------------------------

## Abstract

Trust in the decentralized identity space is a problem that many have tried to solve. This specification aims to take a piece of the problem around [[ref:Trust Establishment]]: a means by which an [[ref:Party]] communicates their assertions for a [[ref:Topic]] about a set of [[ref:Parties]]. [[ref:Trust Establishment]] is intended to be informational, though it can be used to prescribe resulting action. Not all potential usage will be covered in this specification.

## Status of This Document

Trust Establishment is a _STRAWMAN_ specification under development within the Decentralized Identity Foundation (DIF). It incorporates requirements and learning from related work of many active industry players into a shared specification that meets the collective needs of the community.

This specification is regularly updated to reflect relevant changes, and we encourage active engagement on [GitHub](https://github.com/decentralized-identity/trust-establishment/issues) and other mediums (e.g. [DIF Slack](https://difdn.slack.com/archives/C4X50SNUX)).

## Terminology

[[def:Claim, Claims]]
~ An assertion made about a [[ref:Subject]]. Used as an umbrella term for
Credential, Assertion, Attestation, etc.

[[def: Verifiable Credential, VC, VCs]]
~ Refers to the [W3C specification](https://www.w3.org/TR/vc-data-model) of the Verifiable Credentials Data Model.

[[def:Topic, Topics, Trust Topic]]
~ A [[ref:Schema]]-driven document that gives purpose to a trust document. A topic can be anything: from the tangibly measurable (e.g. success rate of a business process) to the intangible (e.g. how one party feels about another).

[[def:Trust Establishment, Trust Establishment Document, Trust Establishment Documents]]
~ The process by which a [[ref:Party]] makes trust statements about a given [[ref:Party]] for a given [[ref:Topic]] using Trust Establishment Documents.

[[def:Identified Parties, Party, Parties]] 
~ Entities that establish their identity by means of a [[def:Decentralized Identifier]]. Parties are commonly referred to as [[ref:Issuers]], [[ref:Holders]], and [[ref:Verifiers]].

[[def:Trust Host, Topic Host, Host]]
~ Entities that are responsible for hosting and communicating [[ref:Trust Establishment Documents]].

[[def:Issuer, Issuers]]
~ Issuers are entities that issue [[ref:Claims]] to [[ref:Holders]], and may be represented as an [[ref:Identified Party]] inside of a [[ref:Trust Establishment Document]].

[[def:Holder, Holders]]
~ Holders are entities that submit [[ref:Claims]] to [[ref:Verifiers]] which contain [[ref:Parties]] that may be identified in a [[ref:Trust Establishment Document]] to satisfy a [[ref:Presentation]] interaction.

[[def:Verifier, Verifiers]]
~ Verifiers are entities that define what [[ref:Parties]] they accept a
[[ref:Credential]] from in order to proceed with an
an interaction.

[[def:Presentation, Present]]
~ Presentations are a type of verifiable interaction in which one [[ref:Party]] submits a [[ref:Claim]] or set of [[ref:Claims]] to a [[ref:Verifier]].

[[def:Decentralized Identifier, Decentralized Identifier, DID]]
~ A [W3C specification](https://www.w3.org/TR/did-core/) describing an _identifier that enables verifiable, decentralized digital identity_.

[[def:Verifiable Interaction, Verifiable Interactions]]
~ An interaction (e.g. request a claim, present a claim, establish trust) which happens between [[ref:Parties]] using Decentralized Identifiers via [[spec:DID-CORE]].

[[def:Schema, Schemas]]
~ A schema is a vocabulary for a [[ref:Claim]]. Commonly used schema formats include [[ref:JSON Schema]] or [[ref:JSON Linked Data]] as surfaced through [schema.org](https://schema.org/).

[[def:Credential Type, Credential Types]]
~ A [[ref:Verifiable Credential]] always has a [_type_ property](https://www.w3.org/TR/vc-data-model/#types) referencing one or more URIs
which provide information semantic meaning and classification for the data in a credential.

## Structure of this Document
This document has two primary sections: In the first we describe the data models for [[ref:Topics]] and [[ref:Trust Establishment Documents]]. In the second, we cover some usage of [[ref: Trust Establishment Documents]].

Examples in this document use the Verifiable Credentials Data Model [[spec:VC-DATA-MODEL]] and the [[spec:DID-CORE]] formats for illustrative purposes only; this specification is intended to support any JSON-serializable [[ref:Claim]] format.

## Data Model

[[ref:Trust Establishment Documents]] are objects that articulate information a [[ref:Party]] publishes in order to share _assertions_ about a set of [[ref:DID]]-identified entities. [[ref:Issuers]], [[ref:Verifiers]], and [[ref:Holders]] may find utility in understanding a [[ref:Party]]'s assertions in making decisions in decentralized web interactions.

There are several document representations aligned around specific organizations of data: _Topic Oriented_, _Entity Oriented_, _Entity Topic Oriented_, and _Set Oriented_. Each representation is optimized around a particular indexing vector, but all contain the necessary information to produce the base data model for each assertion.

### Base Model

The following properties are for use at the top-level of any [[ref:Trust Establishment Document]].

- `id` – The object ****MUST**** contain an `id` property. The value of this property ****MUST**** be a string. The string ****SHOULD**** provide a unique ID for the desired context. For example, a [UUID](https://tools.ietf.org/html/rfc4122) such as `32f54163-7166-48f1-93d8-ff217bdb0653` could provide an ID that is unique in a global context, while a simple string such as `my_trust_establishment-1` could be suitably unique in a local context.
- `author` – The ****MUST**** contain an `id` property. The value of this property ****MUST**** be a string value representing the [[ref:DID]] of the author.
- `created` – The object ****MUST**** contain a `created` property proving a date-time value for when the object was created. The value of this property ****MUST**** be a [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339) compliant timestamp value.
- `validFrom` - The object ****MUST**** contain a `validFrom` property proving a date-time value for when the object is to be used. The value of this property ****MUST**** be a [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339) compliant timestamp value.
- `validUntil` - The object ****MAY**** contain a `validUntil` property proving a date-time value for when the object is no longer to be used. The value of this property ****MUST**** be a [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339) compliant timestamp value.
- `version` – The object ****MUST**** contain a `version` property. The value of this property ****MUST**** be a number. It is recommended that the value is a monotonic increasing integer value.

There are four additional properties common to all Trust Establishment Documents. Depending on the specific formation, the properties are represented in different locations in the document:

- `topic` - A URI property identifying the [[ref:Topic]] of a [[ref:Trust Establishment Document]].
- `entity` - A URI property representing the identity of a party, often represented by a [[ref:DID]], for whom a statement of trust is being made.
- `entries` - An object representing combinations of `topics` and `entities` for trust statements.

::: example Base Model

```json
{
  "id": "32f54163-7166-48f1-93d8-ff217bdb0653",
  "author": "did:example:alice",
  "created": "2022-04-20T04:20:00Z",
  "version": "0.0.3",
  ...
  // topics, entities, and entries below
}
```
:::

### Trust Topic

::: todo text on topics
what's a trust topic?
:::


A topic ****MUST**** be a [[ref:JSON Schema]] document that can be applied to any number of [[ref:Parties]] identified in the `entries` property of a [[ref:Trust Establishment Document]].


**Example JSON Object**

::: example Trust Topic - Full Example

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

### Formations

Each formation builds off of the concepts outlined in the [Base Model](#base-model) and [Trust Topic](#trust-topic) sections of this specification. [[ref:JSON Schema]]s can be found for each formatoin in the [JSON Schema](#json-schema) section.

#### Formation 1: Topic Oriented

This pulls the _topic_ to the top of the document, and indexes properties by _entity_, where the _entity_ happens to be [[ref:DID]].

- `topic` – The object ****MUST**** contain a `topic` property. The value of this property ****MUST**** be a URI identifying the [[ref:Topic]] of the [[ref:Trust Establishment Document]].
- `entries` – The object ****MUST**** contain an `entries` property which is a JSON map and ****MUST**** be composed as a map as follows. 
    - The `entries` object ****MUST**** have map keys as _string_ [[ref:DID]]s which identify [[ref:Parties]] for which trust is being expressed. 
    - The `entries` object ****MUST**** have map values as _JSON objects_ conforming to the [[ref:Schema]] listed in the `topic` property.

::: example Topic Oriented

```json
{
  "id": "32f54163-7166-48f1-93d8-ff217bdb0653",
  "author": "did:example:alice",
  "created": "2010-01-01T19:23:24Z",
  "validFrom": "2010-01-01T19:23:24Z",
  "version": 2,
  "topic": "https://example.com/trusted-supplier.schema.json",
  "entities": {
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

#### Formation 2: Entity Oriented

This pulls the _entity_ to the top of the document, and indexes properties by _entity_, where the _entity_ happens to be a [[ref:Schema]].

- `entity` – The object ****MUST**** contain an `entity` property. The value of this property ****MUST**** be a [[ref:DID]] identifier for a given [[ref:Party]].
- `topics` – The object ****MUST**** contain a `topics` property which is a _JSON map_ and ****MUST**** be composed as a map as follows. 
    - The `topics` object ****MUST**** have map keys as _URIs_ identifying the [[ref:Topic]] of the [[ref:Trust Establishment Document]].
    - The `topics` object ****MUST**** have map values as _JSON objects_ conforming to the associated [[ref:Schema]] key value.

::: example Entity Oriented

```json
{
  "id": "32f54163-7166-48f1-93d8-ff217bdb0653",
  "author": "did:example:alice",
  "created": "2010-01-01T19:23:24Z",
  "validFrom": "2010-01-01T19:23:24Z",
  "validUntil": "2020-01-01T19:23:24Z",
  "version": 2,
  "entity": "did:example:bob",
  "topics": {
    "https://example.com/trusted-supplier.schema.json": {
      "on_time_percentage": 92,
      "goods": ["applewood", "hotel buffet style", "thick cut"]
    },
    "https://example.com/other.schema.json": {
      "foo": "bar"
    }
  }
}
```
:::

#### Formation 3: Entity Topic Oriented

This document specifies neither _entity_ nor _topic_ at the document level, and indexes by _entity_, then _topic_.

- `topics_by_entity` – The object ****MUST**** contain a `topics_by_entity` property which is a _JSON map_ and ****MUST**** be composed as a map as follows. 
    - The `topics_by_entity` object ****MUST**** have map keys as _string_ [[ref:DID]]s which identify [[ref:Parties]] for which trust is being expressed. 
    - The `topics_by_entity` object ****MUST**** have map keys as _JSON objects_, containing _JSON maps_ and ****MUST**** be composed as follows:
      - The nested map inside a `topics_by_entity` object's value ****MUST**** have map keys identifying the [[ref:Topic]] of the [[ref:Trust Establishment Document]].
      - The nested map inside a `topics_by_entity` object's value ****MUST**** have map values as _JSON objects_ conforming to the associated [[ref:Schema]] key value.

::: example Entity Topic Oriented

```json
{
  "id": "32f54163-7166-48f1-93d8-ff217bdb0653",
  "author": "did:example:alice",
  "created": "2010-01-01T19:23:24Z",
  "validFrom": "2010-01-01T19:23:24Z",
  "validUntil": "2020-01-01T19:23:24Z",
  "version": 2,
  "topics_by_entity": {
    "did:example:bob": {
      "https://example.com/trusted-supplier.schema.json":{
        "on_time_percentage": 92,
        "goods": ["applewood", "hotel buffet style", "thick cut"]
      },
      "https://example.com/other.schema.json": {
        "foo": "bar"
      }
    },
    "did:example:carol": {
      "https://example.com/trusted-supplier.schema.json":{
        "on_time_percentage": 74,
        "goods": ["oinkys", "porkys", "wilburs"]
      },
      "https://example.com/other.schema.json": {
        "foo": "baz"
      }
    }
  }
}
```
:::

#### Formation 4: Topic Entity Oriented

This document specifies neither _entity_ nor _topic_ at the document level, and indexes by _entity_, then _topic_.

- `entities_by_topic` – The object ****MUST**** contain a `topics_by_entity` property which is a _JSON map_ and ****MUST**** be composed as a map as follows. 
    - The `entities_by_topic` object ****MUST**** have map keys as _string_ values identifying the [[ref:Topic]] of the [[ref:Trust Establishment Document]]. 
    - The `entities_by_topic` object ****MUST**** have map keys as _JSON objects_, containing _JSON maps_ and ****MUST**** be composed as follows:
      - The nested map inside a `entities_by_topic` object's value ****MUST**** have map keys as [[ref:DID]]s which identify [[ref:Parties]] for which trust is being expressed. 
      - The nested map inside a `entities_by_topic` object's value ****MUST**** have map values as _JSON objects_ conforming to the associated [[ref:Schema]] key of the parent `entity_by_topic` value.


::: example Topic Entity Oriented

```json
{
  "id": "32f54163-7166-48f1-93d8-ff217bdb0653",
  "author": "did:example:alice",
  "created": "2010-01-01T19:23:24Z",
  "validFrom": "2010-01-01T19:23:24Z",
  "validUntil": "2020-01-01T19:23:24Z",
  "version": 2,
  "entities_by_topic": {
    "https://example.com/trusted-supplier.schema.json": {
      "did:example:bob": {
        "on_time_percentage": 92,
        "goods": ["applewood", "hotel buffet style", "thick cut"]
      },
      "did:example:carol": {
        "on_time_percentage": 74,
        "goods": ["oinkys", "porkys", "wilburs"]
      }
    },
    "https://example.com/other.schema.json":{
      "did:example:bob": {
        "foo": "bar"
      },
      "did:example:carol": {
        "foo": "baz"
      }
    }
  }
}
```
:::


#### Formation 5: Set Oriented

This document specifies neither _entity_ nor _topic_ at the document level, and lists base model structures inside _entity_.

- `set` – The object ****MUST**** contain a `set` property which is a JSON map and ****MUST**** be composed as a _JSON array_ as follows:

    - The `set` object ****MUST**** contain a `topic` property. This value ****MUST**** be a _string_ identifying the [[ref:Topic]] of the [[ref:Trust Establishment Document]]. 
    - The `set` object ****MUST**** contain an `entity` property. This value ****MUST**** be a [[ref:DID]]s which identify [[ref:Parties]] for which trust is being expressed. 
    - The `set` object ****MUST**** contain a `properties` property. This value ****MUST**** be a _JSON object_ conforming to the associated [[ref:Schema]] referenced in the corresponding `topic` property's value.

::: example Set Oriented

```json
{
  "id": "32f54163-7166-48f1-93d8-ff217bdb0653",
  "author": "did:example:alice",
  "created": "2010-01-01T19:23:24Z",
  "validFrom": "2010-01-01T19:23:24Z",
  "version": 2,
  "set": [
    {
      "topic": "https://example.com/trusted-supplier.schema.json",
      "entity": "did:example:bob",
      "properties": {
        "on_time_percentage": 92,
        "goods": ["applewood", "hotel buffet style", "thick cut"]
      }
    },
    {
      "topic": "https://example.com/other.schema.json",
      "entity": "did:example:bob",
      "properties": {
        "foo": "bar"
      }
    },
    {
      "topic": "https://example.com/trusted-supplier.schema.json",
      "entity": "did:example:carol",
      "properties": {
        "on_time_percentage": 74,
        "goods": ["oinkys", "porkys", "wilburs"]
      }
    },
    {
      "topic": "https://example.com/other.schema.json",
      "entity": "did:example:carol",
      "properties": {
        "foo": "baz"
      }
    }
  ]
}
```
:::


## Document Integrity

[[ref:Trust Establishment Documents] may fit into any number of embed targets – verifiable wrappers or embedded proofing formats - to enable data integrity. This specification takes no position on which means are suitable to provide integrity of [[ref:Trust Establishment Documents]] or [[ref:Topics]], however provide a number of examples for convenience:

### Verifiable Credential

**Linked Data Proof Verifiable Credential Trust Establishment Document**

::: example Linked Data Proof Verifiable Credential Trust Establishment Document

```json
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://www.w3.org/2018/credentials/examples/v1"
  ],
  "id": "http://example.edu/credentials/3732",
  "type": ["VerifiableCredential", "TrustEstablishment", "TrustedSuppliers"],
  "issuer": "did:example:alice",
  "issuanceDate": "2010-01-01T00:00:00Z",
  "credentialSubject": {
    "id": "did:example:ebfeb1f712ebc6f1c276e12ec21",
    "trustEstablishment": { 
    // Is this still needed?
	  "id": "32f54163-7166-48f1-93d8-ff217bdb0653",
	  "author": "did:example:alice",
    // Is this still needed?
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
  },
  "proof": {
  	"type": "Ed25519Signature2020",
    "created": "2021-11-13T18:19:39Z",
    "verificationMethod": "did:example:alice#key-1",
    "proofPurpose": "assertionMethod",
    "proofValue": "z58DAdFfa9SkqZMVPxAQpic7ndSayn1PzZs6ZjWp1CktyGesjuTSwRdoWhAfGFCF5bppETSTojQCrfFPP2oumHKtz"
  }
}
```

:::


### JSON Web Token (JWT)

**JWT-VC Verifiable Credential Trust Establishment Document**

::: example JWT-VC Verifiable Credential Trust Establishment Document - Header

```json
{
    "alg": "EdDSA",
    "typ": "JWT",
    "kid": "did:alice#keys-1"
}
```

:::

::: example JWT-VC Verifiable Credential Trust Establishment Document - Body

```json
{
  "sub": "did:example:alice",
  "jti": "http://example.com/credentials/3732",
  "iss": "did:alice#keys-1",
  "nbf": 1541493724,
  "iat": 1541493724,
  "exp": 1573029723,
  "nonce": "660!6345FSer",
  "vc": {
    "@context": [
      "https://www.w3.org/2018/credentials/v1",
      "https://www.w3.org/2018/credentials/examples/v1"
    ],
    "type": [
      "VerifiableCredential",
      "TrustEstablishment",
      "TrustedSuppliers"
    ],
    "credentialSubject": {
      "id": "did:example:ebfeb1f712ebc6f1c276e12ec21",
      "trustEstablishment": {
        "id": "32f54163-7166-48f1-93d8-ff217bdb0653",
        "author": "did:example:alice",
        "created": "2010-01-01T19:23:24Z",
        "version": "0.0.3",
        "topic": "https://example.com/trusted-supplier.schema.json",
        "entries": {
          "did:example:bob": {
            "on_time_percentage": 92,
            "goods": [
              "applewood",
              "hotel buffet style",
              "thick cut"
            ]
          },
          "did:example:carol": {
            "on_time_percentage": 74,
            "goods": [
              "oinkys",
              "porkys",
              "wilburs"
            ]
          }
        }
      }
    }
  }
}
```
:::

::: note
The third component of the JWT, the signature in JWS form is not shown. You can imagine it being a "third" component of the JWT above.
:::

## Publication & Discovery


While specific recommendations for how to host, publish, and discover Trust Establishment documents is out of scope of this specification we can raise a number of points for consideration for its users.

The publication of the JSON-based documents defined in this specification is a common activity users of the specification will need to engage in for their expressions of trust and sentiment to be discovered and digested by other parties. There are a number of ways to do this, but they should account for the following core abilities:

1. Enable [[ref:Trust Establishment]] documents to be located via crawling a known set of DIDs (via [Service Endpoints](https://www.w3.org/TR/did-core/#services)) or some other common routing mechanisms.
2. Enable querying for all or some subset of [[ref: Trust Establishment]] documents from a target entity via common DID-based data query/interaction protocols.

There may be different motivations for interacting with [[ref:Trust Establishment]] documents. Across those motivations there are a common set of questions one may ask themselves before publishing, including:

- Who are the intended consumers of this document, and how might they become aware of it?
- Is this document going to be updated? If so, how are updates shared? How may consumers of older versions become aware that there is a more recent version?
- Is hosting this document with a single party a risk: to availablility, privacy erosion, or censorship in some manner?
- If I intend for this document to be replicated, how do I do that? Is there a way to incentivize other parties to replicate my documents?

## Appendix

### Topic Registry

::: todo Topic Registry
  Create a place for topics to be registered.
:::

### JSON Schema

The [[ref:Trust Establishment]] specification adopts and defines the following [[ref:JSON Schema]] data format and processing variant, which implementers ****MUST**** support for evaluation of the portions of the specification that call for [[ref:JSON Schema]] validation: https://json-schema.org/draft/2020-12/json-schema-core.html

#### Topic Oriented

::: example Topic Oriented - Schema
```json
[[insert: ./schemas/topic-oriented.json]]
```
:::

#### Entity Oriented

::: example Entity Oriented - Schema
```json
[[insert: ./schemas/entity-oriented.json]]
```
:::

#### Entity Topic Oriented

::: example Entity Topic Oriented - Schema
```json
[[insert: ./schemas/entity-topic-oriented.json]]
```
:::

#### Topic Entity Oriented

::: example Topic Entity Oriented - Schema
```json
[[insert: ./schemas/topic-entity-oriented.json]]
```
:::

#### Set Oriented

::: example Set Oriented - Schema
```json
[[insert: ./schemas/set-oriented.json]]
```
:::

### Examples

#### Topic Oriented

**Example #1 - Sentiment Declaration**

Sentiment Declaration is a usage of the [[ref:Trust Establishment]] data model that provides a means by which an [[ref:Entity]] communicates their sentiment (what they may feel or think) for a [[ref:Topic]] about a set of [[ref:Parties]]. Sentiment Declaration is intended to be informational, and the prescription of any resulting actions taken based on the contents of the document are out of scope of this specification.

As an example, we think of "My Faves". The year is 2007 and your pink-colored mobile carrier announces a program to let you make unlimited calls and texts to five contacts of your choosing: your faves. Using the [[ref:Trust Establishment]] specification, you create a [[ref:JSON Schema]] [[ref:Topic]] that supports you enumerating your faves.

::: example Sentiment of "My Faves" :::

```json
[[insert: ./examples/topic-oriented/example-1-schema.json]]
```

:::

Next, you take the "My Faves" schema, and put it in a Trust Establishment document, which you can share with your mobile carrier, or post it on social media if you're feeling bold.

::: example Trust Establishment Sentiment using the "My Faves" Topic

```json
[[insert: ./examples/topic-oriented/example-1.json]]
```

:::

You may imagine your carrier similarly using the "My Faves" schema to enumerate favs for all customers, each a separate entity within the [[ref:Trust Establishment Document]].

**Example #2 – Trusted Issuers**

A common usage of [[ref:Trust Establishment Documents]] is by a [[ref:Verifier]] wishing to provide information on which [[ref:Credentials]] they accept from which [[ref:Issuers]]. Using [[ref:Verifiable Credentials]] we know that a given Credential may have a signle schema, but also may reference multiple schemas or `type` documents for the same Credential. Using a "Trusted Issuers for Credential" topic, we can fulfill this use case.

::: example Trusted Issuers for Credential :::

```json
[[insert: ./examples/topic-oriented/example-2-schema.json]]
```

:::

Next, you take the "Trusted Issuers For Credential" schema, and put it in a Trust Establishment document, which you can share with any relying party aiming to seek your verification services.

::: example Trust Establishment using the "Trusted Issuers for Credential" Topic

```json
[[insert: ./examples/topic-oriented/example-2.json]]
```

:::

#### Entity Oriented

**Example 1 - Multiple Topics for Entity**

Alice is publishing a list about herself, sharing information that could be used in interactions with her.

::: example Entity Oriented Example
  ```json
[[insert: ./examples/entity-oriented/example-1.json]]
```
:::


#### Entity Topic Oriented

**Example 1 - Government Flight Regulations**

In this example, the government identified by `did:example:government` is authoring a document showing for which airlines (the given entities) the qualifications needed for each of their pilots: information about a requisite flight school and medical report.

::: example Entity Topic Oriented Example
  ```json
[[insert: ./examples/entity-topic-oriented/example-1.json]]
```
:::

#### Topic Entity Oriented

**Example 1 - Accepted University Degrees**

In this example, the author is an employer identified by `did:example:employer-1` who is listing which degrees they are willing to accept for applicants from a set of universities.

::: example Topic Entity Oriented Example
  ```json
[[insert: ./examples/topic-entity-oriented/example-1.json]]
```
:::


#### Set Oriented

**Example 1 - Pizza Store**

A piza store, identified by `did:example:round-n-proud` is publishing a list of their chosen vendors, including which products they are willing to purchase for each vendor. This is useful for the vendors, and fans of _Round n' Proud_ everywhere that want to try to re-create the pizza at home.

::: example Set Oriented Example
  ```json
[[insert: ./examples/set-oriented/example-1.json]]
```
:::

## References

[[def: JSON Schema]]
  - [JSON Schema](https://json-schema.org/)
  - [JSON Schema: A Media Type for Describing JSON Documents](https://json-schema.org/draft/2020-12/json-schema-core.html). A. Wright, H. Andrews, B. Hutton, G. Dennis. Status: 28 January 2020. Internet-Draft.

[[def: JSON-LD, JSON Linked Data]]
  - [JSON-LD](https://json-ld.org/)
  - [JSON-LD 1.1, _A JSON-based Serialization for Linked Data_](https://www.w3.org/TR/json-ld/). Manu Sporny, Dave Longley, Gregg Kellogg, Markus Lanthaler, Pierra-Antoine Champin, Niklas Lindström. 16 July 2020. Status: W3C Recommendation.

[[def: Linked Data Proofs, Data Integrity]]
  - [Data Integrity 1.0](https://w3c-ccg.github.io/data-integrity-spec/). Dave Longley, Manu Sporny. 30 April 2022. Status: W3C Draft Community Report.

[[spec]]