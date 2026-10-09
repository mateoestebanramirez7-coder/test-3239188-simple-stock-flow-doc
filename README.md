# test-3239188-simple-stock-flow-doc

> **SDD Challenge · ADSO Class 3239188**
> Delivery: **today, October 8, 2026, at 10:50 PM (Colombia Time)**. Countdown: https://claude.ai/artifact/3ja3TMwGnprBV6QCcyAruT

This challenge **is not performed in your project's main repository**. It is done in a **fork of this repository**.

## What to do

The only input is the *Simple Stock Flow* data model: [`spec/data-model.md`](spec/data-model.md).
From it, the system documentation is reconstructed **backwards**, from architecture to context:

| Order | Directory | What is produced from the data model |
|---|---|---|
| 1 | `05-architecture/` | System style and components implied by the model (aggregates, ports, where each rule lives) |
| 2 | `04-requirements/` | User stories and non-functional requirements necessitated by the model |
| 3 | `03-product/` | Problem solved and product vision |
| 4 | `02-domain/` | Domain entities, rules, and events, along with its glossary |
| 5 | `01-context/` | General description and scope (what is built and what is not) |
| 6 | `05-architecture/` (closing) | Return to architecture and verify that it aligns with everything above and with the model |

The `06-data/` directory **is not written**: it is the provided model.

## Rules

1. Create a **fork** of this repository to your personal or team account.
2. Work in your fork. One folder per document, using the names from the table above.
3. Every assertion must be traceable to the data model (cite the section, e.g., "§2.3" or "FK-2").
   If something does not originate from the model, mark it as an **assumption**.
4. What counts is the **last commit prior to 10:50 PM**. Anything pushed afterward will not be reviewed.
5. Performance is evaluated on SDD usage: how the specification is read, interpreted, and applied. Not the volume of text.

## Team Progress Status

Calculated by comparing each `-docs` repository against the governance template. Weeks correspond to `00-sdd-guide.md`: week 1 context and domain (01-02), week 2 product and requirements (03-04), weeks 2-3 architecture and data (05-06), weeks 3-4 detailed design (07 onwards).

| Team (project) | Current Week | Current Status |
|---|---|---|
| lexia | 3-4 | 01 to 06 and 07-api |
| fixgo | 2-3 | 01 to 06 |
| smart-technical-service-to-professional | 2-3 | 01 to 06 |
| belleza-ya | 2-3 | 01 to 06 |
| distrilink | 2 | 01 to 04; missing 05 |
| interemprendedores | 2 | 01 to 04; missing 05 |
| construction-project-management-system | 1 | 01-context only |
| residential-complex | not started | no changes over template |
| huila-travel-expedition | not started | no changes over template |
| huila-travel-services | not started | no changes over template |
| fastbill-manager | not started | no changes over template |

This is an estimate based on modified files in `main`. If your team worked on a different branch, please report it.

## Note on the Data Model

`spec/data-model.md` links to other documents from the original spec (`constitution.md`, `plan.md`, `adr/`, etc.).
**They are not provided**: those links will not open. Everything you need is contained within the model.