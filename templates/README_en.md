# Template Conversion Guide

This repository contains a Velocity template conversion for the **Yimi CRUD Generator** IntelliJ IDEA plugin, derived from the JHipster CRUD generator EJS templates. The purpose is to help users understand the conversion approach, the simplifications made, and the areas still requiring further work.

## Purpose and Scope

The goal of this template set is to:

- Migrate JHipster EJS generation logic to Velocity templates
- Preserve core CRUD generation flow
- Support Yimi CRUD Generator configuration and conditional template generation
- Provide readable initialization logic and comments for maintainability

The current implementation focuses on common entity CRUD scenarios and does not yet cover all JHipster features or every plugin environment variable.

## Structure and File Overview

- `config.xml`
  - Defines template mapping rules
  - Maps source template files (`sourcePath`) to target generated files (`targetPath`)
  - Supports conditional generation via `condition` and template grouping via `profile`

- `init.vtl`
  - Acts as a single entry point for constructing shared context variables
  - Translates original JHipster EJS logic into Velocity syntax
  - Handles entity fields, relationships, primary keys, pagination, search, DTO settings, service layer flags, and i18n paths

- `backend/`, `spring-boot/`, `client/_vue/`
  - Contain the actual source template files
  - Template content keeps JHipster-style logic but is written in Velocity

## Detailed Conversion Approach

### 1. Unified Context Construction

The main task of `init.vtl` is to compute complex logic up front and expose reusable variables for later templates.

Key responsibilities include:

- Mapping `global` configuration options
  - e.g. `global.searchEngine`, `global.reactive`, `global.service`, `global.entitySpringPreAuthorizePrefix`
- Extracting primary key metadata
  - Detect primary key type, field name, getter/setter naming
  - Support special handling for `UUID` and `String` primary keys
- Deriving field properties
  - Compute `fieldType`, `fieldNameHumanized`
  - Detect blob or time-derived fields with `anyFieldIsBlobDerived` and `anyFieldIsTimeDerived`
- Building relationship structures
  - Parse `nestInfo` for one-to-one, one-to-many, many-to-one, many-to-many relations
  - Produce `relationships`, `relationshipsByOtherEntity`
  - Handle owner/owned side, bag relationships, eager loading logic
- Configuring DTO and service layer behavior
  - Detect `resultModelClass.classType == 'DTO'` and set MapStruct/DTO mode
  - Set flags like `serviceNo` and `serviceImpl`
- Computing Spring Data mode
  - Set `springDataDescription` based on `global.databaseType` and `global.reactive`
- Generating frontend path and i18n keys
  - Compute `entityFileName`, `entityFolderName`, `entityTranslationKey`
  - Simplify AngularJS suffix and path normalization logic

This “compute first, use later” approach keeps individual templates simpler and consistent.

### 2. Conditional Generation and Plugin Compatibility

`config.xml` is the plugin integration layer that tells Yimi CRUD Generator:

- which source template should generate which target file
- when to generate the template based on conditions
- which templates are grouped under a profile

Examples:

- Update page templates are only generated when `readOnly == false`
- Reactive repositories are generated when `global.reactive` is true
- DTO and mapper templates are generated when `resultModelClass.classType == 'DTO'`

The frontend configuration uses an `admin` profile and targets the `src\main\webapp` base directory.

### 3. Simplifications and Practical Decisions

To make migration feasible, the conversion intentionally simplifies some JHipster behaviors:

- `generator.embedded` is fixed to `false`
  - Embedded entities and `@Embeddable` structures are not handled currently
- `entityPersistenceLayer` is fixed to `true`
  - Backend persistence layer generation always follows standard JPA/Repository patterns
- AngularJS suffix and legacy path logic are simplified
  - Variables like `entityAngularJSSuffix`, `normalizePathEnd`, and `entityStateName` are not separately computed
- Microservice paths are normalized to `services/${global.microserviceName.toLowerCase()}/`
  - Only common microservice path structure is retained
- i18n prefix is generated as `global.appName + entityFileName`
- Some historical JHipster branches are not fully preserved
  - e.g. old `microserviceAppName` handling, advanced AngularJS name suffixes, complex form validation branches

These simplifications preserve the most common CRUD generation paths while avoiding overwhelming template complexity.

## Supported Plugin Variables

The conversion currently processes the following plugin variables in `init.vtl`:

- `global.clientRootFolder`
- `readDefinition.paginationInfiniteScroll`
- `deleteDefinition`
- `createDefinition`, `updateDefinition`, `deleteDefinition`
- `requestModelClass` / `resultModelClass`
- `currentModel.fields`
- `global.reactive`
- `global.entityR2dbcRepository`
- `global.searchEngine`
- `global.service`
- `global.entitySpringPreAuthorizePrefix`
- `global.databaseType`
- `global.applicationType`
- `global.authenticationType`

These variables are normalized into reusable context values that later templates can reference consistently.

## How to Continue Improving

To continue refining the template set, prioritize these areas:

1. Add embedded entity support
   - `embedded` is currently hardcoded to `false`
   - Implement nested field generation and embeddable relations
2. Improve frontend route and filename rules
   - Restore or adapt original `entityAngularJSSuffix` and `entityStateName` behavior
3. Expand `config.xml` conditions
   - Add more profiles, conditional generation rules, or output targets
   - Support additional frontend/backend variants as needed
4. Validate more JHipster scenarios
   - Multi-select relationships, composite keys, DTO + criteria queries, Elasticsearch, Reactive mode
5. Verify actual plugin environment variables
   - Confirm the full `global`, `currentModel`, and `readDefinition` attribute set from Yimi CRUD Generator

## AI Assistance Note

This template conversion was assisted by AI tooling during analysis and documentation.

- AI helped map original JHipster EJS logic to Velocity
- AI generated `init.vtl` variable derivation logic and comments
- AI also helped summarize the conversion strategy and simplification decisions

As a result, this template is currently in a “semi-automated migration” state: it supports the core CRUD flow, but still requires manual review, testing, and refinement.

---

This document is intended to help maintainers understand the conversion approach and the current implementation boundaries, so that they can continue improving the template set.