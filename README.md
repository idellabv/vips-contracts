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

Every schema has exactly one owning service, and through it an owning team. A schema used by
more than one service still has one owner; the other services are its consumers.

Ownership is recorded in the internal source of these schemas. It is **not** published: the
internal ownership annotations are stripped from every file in this repository. For a question
or a change request about a contract, contact [Idella](mailto:info@idella.com) and name the
schema's path.

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

## How schemas get here

The schemas here are curated for reuse: the shared entities of the VIPS APIs and events (policies,
coverages, dossiers, tasks and their enums), in JSON Schema draft 2020-12. They are maintained in an
internal contracts repository, and only the subset intended for reuse is published here. OpenAPI
documents, response building blocks and internal-only schemas are not published.

Every release follows the same steps:

1. A release is tagged in the internal repository.
2. Automated checks run: the published set must be self-contained, and nothing internal may leak
   (hosts, names, ticket keys or e-mail addresses).
3. The release is approved.
4. A pull request is opened here from a **new branch**.
5. A person reviews and merges the pull request. Nothing is pushed or merged automatically.

Each release adds new version directories. Existing versions are never modified.

## Using the schemas

Reference a schema by its URI, which is also its `$id`:

```
https://raw.githubusercontent.com/idellabv/vips-contracts/main/pensions/json-schemas/<product>/<entity>/<version>/<entity>.schema.json
```

For example, the Task entity:
`https://raw.githubusercontent.com/idellabv/vips-contracts/main/pensions/json-schemas/taskmgmt/task/1.1.0/task.schema.json`.

- **Find the current versions** in [`pensions/json-schemas/published-index.json`](pensions/json-schemas/published-index.json).
  It lists every published entity with its name, version, path and URI. Take URIs from this index
  rather than building them by hand.
- **Pin a version.** Version directories are immutable, so a pinned URI never changes under you.
  Move to a new version when you choose to; the versioning rule above tells you whether the move
  can break you.
- References between published schemas are relative, so a local checkout of this repository
  resolves on its own, without network access.
