# Contract First

[Demo](https://github.com/cleitus/contract-first-demo)

# Intro

Decathlon microservices platform

Contrat everywhere dans le SI.

OpenAPI history, 2010, createur de swagger, 2021 json.

**Code first approach VS Contract first approach**

Code first : Design > Implementation > Schema
Contract first : Design > Schema > Implementation

## Pros Cons

**Pros** :
 - Reusability
 - Versioning
 - Loose coupling
 - // development
 - co-build design
 - Governance

**Cons** : 
 - Dev exp
 - Additional design cost
 - Need to learn JSON Schema
 - Rigidity

OpenAPI -> Implementation (openapi-generator.tech)
[OpenAPI installation](https://openapi-generator.tech/docs/installation)

# Demo

## Server code

Spring boot generator, creer 3 dir : `api, configuration, model` 

## Client code

resttemplate generator, creer 2 dirs : `api, auth` et apres des files de conf

Il a creer un utilitaire ApiClient aui permet de dialoguer avec le serveur


Schema | Schematesis | Implementation

# Take Away

 - Clients : Client generators 50+
 - Server development
 - Workflow integration : Immutable and ephemeral
 - Ensure schema compliance
 - Generate static doc
 - Active community