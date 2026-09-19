# Project Delivery and Jira Evidence

## Purpose

This page provides a public, reviewable view of how the FX Forward Lifecycle Integration case study is planned, delivered, validated, and traced across Jira, ADONIS, and GitHub.

The Jira workspace is private and remains the working source of truth for backlog state, sprint assignment, issue relationship, ownership, and delivery comments. This repository contains selected sanitised evidence rather than a manually maintained copy of the entire Jira backlog.

> **Portfolio notice:** The project, Jira issues, delivery records, and supporting artefacts form part of ficional portfolio case study. They do not represent delivery for a real organisation or client.

## Delivery Toolchain

| Tool | Use in the project |
|---|---|
| Jira | Backlog management, sprint planning, prioritisation, ownership, estimates, status, and completion evidence |
| Confluence | Selected working documentation and analysis development |
| ADONIS | BPMN 2.0 modelling, element descriptions, model relationships, and syntax validation |
| GitHub | Version control, public artefact publication, commit history, and technical traceability |

## Working Model

Delivery work is decomposed from broader analysis outcomes into reviewable increments. A BPMN delivery item normally covers one bounded Level 2 process model or one defined package-level activity.

Each increment follows the same operating pattern:

1. select and confirm the bounded scope from the backlog;
2. model the happy path and applicable exception or recovery paths;
3. add roles, systems, data objects, data stores, and traceability descriptions;
4. connect the Level 2 process to the Level 1 lifecycle model where applicable;
5. run the available ADONIS validation checks;
6. export and inspect the editable BPMN source;
7. update the model inventory and supporting documentation;
8. commit and push the repository changes;
9. record implementation and validation evidence in Jira.

## Definition of Done for a BPMN Delivery Item

A current-state BPMN item is considered delivered when:

- the agreed process boundary and source activity identifiers are represented;
- the standard path, relevant devisions, exceptions, recovery loops, and terminal outcomes are modelled;
- stakeholder responsibilities and current-state system activities are assigned to appropriate lanes;
- material inputs, outputs, persistent stores, and data associations are represented;
- element descriptions contain the required source and traceability references;
- the Level 1 Call Activity physically references the applicable Level 2 model where required;
- affected model pass the available ADONIS checks without unresolved modelling errors or warnings;
- exported BPMN sources are well-formed XML and pass the selected repository checks;
- the current-state model inventory is updated;
- repository changes are commited and pushed;
- Jira contains the repository path, commit hash, delivered scope, and validation evidence.

The artefacts remaint at **Draft** maturity until the complete model package has undergone formal portfolio review. Delivery completion and document maturity are therefore recorded separately.

## Delivered Increments

| Increment | Repository outcome | Public Git evidence |
|---|---|---|
| Current-state process baseline | End-to-end current-state narrative, process inventory, controls, pain points, and validation register | [`aef34bb`](https://github.com/lbakiewicz99/fx-forward-lifecycle-integration-ba/commit/aef34bb) |
| BPMN hierarchy and conventions | Model hierarchy, notation rules, traceability requirements, and repository conventions | [`aa19156`](https://github.com/lbakiewicz99/fx-forward-lifecycle-integration-ba/commit/aa19156) |
| Level 1 lifecycle model | `CSP-BPMN-L1-001` covering `CSP-PR-001` - `CSP-PR-008` | [`cc94013`](https://github.com/lbakiewicz99/fx-forward-lifecycle-integration-ba/commit/cc94013) |
| Level 2 new-trade booking | `CSP-BPMN-L2-001` | [`127ce05`](https://github.com/lbakiewicz99/fx-forward-lifecycle-integration-ba/commit/127ce05) |
| Level 2 amendment and cancellation | `CSP-BPMN-L2-002` | [`9c18133`](https://github.com/lbakiewicz99/fx-forward-lifecycle-integration-ba/commit/9c18133) |
| Level 2 daily valuation | `CSP-BPMN-L2-003`, including exception handling and the physical Level 1 reference | [`e775e6b`](https://github.com/lbakiewicz99/fx-forward-lifecycle-integration-ba/commit/e775e6b), [`5963c09`](https://github.com/lbakiewicz99/fx-forward-lifecycle-integration-ba/commit/5963c09), [`b92c355`](https://github.com/lbakiewicz99/fx-forward-lifecycle-integration-ba/commit/b92c355) |

## Jira-to-Repository Traceability

The following table records selected work items that are useful for public review. Jira issue pages are intentionally not linked because the workspace is private.

| Jira item | Delivery role | Repository artefact | Completion evidence |
|---|---|---|---|
| `FXF-23` | Current-state BPMN package | [BPMN package index](../02-process-and-architecture/diagrams/current-state/README.md) | Package inventory and incremental Git history |
| `FXF-29` | Coordination of the agreed Level 2 model package | [Level 2 source directory](../02-process-and-architecture/diagrams/current-state/source) | In progress; completed models recorded separately |
| `FXF-31` | Complete and validate Level 2 amendment and cancellation BPMN model | [`CSP-BPMN-L2-002`](../02-process-and-architecture/diagrams/current-state/source/csp-bpmn-l2-002-process-amendment-and-cancellation.bpmn) | Commit [`9c18133`](https://github.com/lbakiewicz99/fx-forward-lifecycle-integration-ba/commit/9c18133) |
| `FXF-32` | Complete and validate Level 2 daily valuation BPMN model | [`CSP-BPMN-L2-003`](../02-process-and-architecture/diagrams/current-state/source/csp-bpmn-l2-003-process-daily-valuation.bpmn) | Commits [`e775e6b`](https://github.com/lbakiewicz99/fx-forward-lifecycle-integration-ba/commit/e775e6b), [`5963c09`](https://github.com/lbakiewicz99/fx-forward-lifecycle-integration-ba/commit/5963c09), and [`b92c355`](https://github.com/lbakiewicz99/fx-forward-lifecycle-integration-ba/commit/b92c355) | 

## Selected Jira Evidence

The public evidence set will contain a small number of sanitised screenshots showing:

| Planned evidence file | What it demonstrates |
|---|---|
| `evidence/jira/sprint-3-board-completed.png` | Sprint goal, bounded scope, estimate, ownership, and completed delivery state |
| `evidence/jira/fxf-32-details.png` | Work-item definition, status, spring assignment, priority, and estimate |
| `evidence/jira/fxf-32-completion-evidence.png` | Delivered scope, validation result, repository path, and commit hashes |
| `evidence/jira/fxf-29-progress.png` | Package-level progress across the remaining Level 2 models |

Images are added only after irrelevant workspace information has been cropped and the displayed status is consistent with the repository state. No screenshot is treated as replacement for version-controlled artefact or Git history.

## Traceability Principles

- A Jira status does not by itself prove that an artefact was delivered.
- A diagram is not complete solely because the happy path exists.
- Repository paths and commit hashes provide immutable delivery references.
- Jira comments record the delivered scope and validation result in business-readable language.
- Document maturity is kept separate from completion of the Jira delivery item.
- Planned work is not presented as completed portfolio evidence.

## Current Delivery Snapshot

At the completion of `FXF-32`:

- the Level 1 current-state lifecycle model exists and references the completed Level 2 models;
- `CSP-BPMN-L2-001`, `CSP-BPMN-L2-002`, and `CSP-BPMN-L2-003` are repository-published in Draft state;
- three of seven planned Level 2 models have been delivered;
- the remaining Level 2 package continues under `FXF-29`;
- final package validation and publication remain separate planned work.

The current model inventory is maintained in the [Current-State BPMN Package README](../02-process-and-architecture/diagrams/current-state/README.md).