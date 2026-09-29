# vips-contracts
Idella BV public resources, OpenAPI and other SCHEMA specifications

👤 [Idella](mailto:info@idella.com)

This repository contains the data contracts for the VIPS product lines. It serves as the source of truth for the public-facing APIs and events, structured by product line.

## 📖 Introduction

This repository is the central location for all data contracts related to the VIPS product lines. It is designed to be the single source of truth that drives consistency across our services. The repository is organized by product line, with each line containing its own set of `json-schemas`, `open-api`, and `cloudevents` definitions.

### Repository Structure

The repository is organized by product line, starting with `pensions`. Each product line folder contains subfolders for the different types of contracts:
```
/
└── pensions/
    ├── json-schemas/
    │   ├── generic/
    │   │   └── name-of-the-entity/
    │   │       └── 1.0.0/
    │   │           ├── name-of-the-entity.schema.json
    │   │           └── name-of-the-entity.example.json
    │   └── microservice-a/
    │       └── name-of-the-entity/
    │           └── 1.0.0/
    │               ├── name-of-the-entity.schema.json
    │               └── name-of-the-entity.example.json
    ├── open-api/
    │   └── name-of-the-api/
    │       └── v1.0/
    │       │   └── name-of-the-api.oas.yaml
    │       └── v1.1/
    │       │   └── name-of-the-api.oas.yaml
    │       └── v2.0/
    │           └── name-of-the-api.oas.yaml    
    └── cloudevents/
        ├── internal-events/
        │   └── group-name/
        │       └── v1.0/
        │       │   └── entity-name-data.schema.json
        │       │   └── entity-name-data.simple-example.json
        │       │   └── entity-name-data.complex-example.json
        │       └── v1.1/
        │       │   └── entity-name-data.schema.json
        │       │   └── entity-name-data.simple-example.json
        │       └── v2.0/
        │           └── entity-name-data.schema.json
        │           └── entity-name-data.simple-example.json
        │           └── entity-name-data.complex-example.json
        └── external-events/
            └── external-group-name/
                └── v1.0/
                    └── entity-name-data.schema.json
                    └── entity-name-data.simple-example.json
                    └── entity-name-data.complex-example.json
    
```

### Standards

All contracts within this repository adhere to the following standards:

* **File and Folder Naming**: All file and folder names use `kebab-case`.
* **JSON Schema Properties**: Properties are written in `lower_snake_case`.
* **Enums**: Enum values are written in `UPPER_SNAKE_CASE`.
* **OpenAPI Version**: All `open-api` specifications are written in OAS 3.1.
* **JSON Schema Versioning**: For `json-schemas`, each entity has its own folder. Inside, a subfolder for each semantic version (e.g., `1.0.0`) contains the `name-of-the-entity.schema.json` and a corresponding `name-of-the-entity.example.json` file.
* **Grouping of JSON Schemas**: json-schemas are grouped based on the microservice that is the source of truth for them

## Ownership model

Every schema in this repository has a declared owner, so anyone reading a contract can
find out who to ask about it and where to raise a change request:

* **Owning service** — the microservice that is the source of truth for the schema. A
  schema used by more than one service still has exactly one owning service; the other
  services are listed as consumers alongside it.
* **Owning team** — the engineering team responsible for that service.
* **Jira project** — the Jira project the owning team files and tracks work under, for
  anyone who wants to raise a question or a change against a specific contract.

A schema's owner fields travel with it wherever the schema is published; this repository
never restates ownership separately from the schema file itself.

## Versioning rule

Each entity is versioned independently with semantic versioning, and the version bump is derived
mechanically from the change rather than chosen by hand:

* **Major** — the change narrows what the schema accepts (a property removed or newly required, a
  type narrowed, an enum value removed, a constraint added or tightened).
* **Minor** — the change widens what the schema accepts (an optional property added, an enum value
  added, a constraint removed or loosened).
* **Patch** — annotations only (descriptions, titles, examples, format hints).

Earlier versions stay in place next to the new one, so a consumer pinned to a version keeps
working.

## Publish plan

The schemas in this repository are curated for public consumption: reviewed, versioned,
and free of any internal implementation detail. They are authored and maintained in a
private, internal contracts repository first, and only the subset intended for external
consumers is published here.

Today, that publishing step is a manual review and copy. A future automated pipeline
(tracked internally) is planned to keep this repository in sync with the private source
on every release, without requiring a manual copy step. Nothing in this repository is
published automatically at this time — every file here has been through a review.
