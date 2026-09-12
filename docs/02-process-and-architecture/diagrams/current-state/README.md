# FX Forward Current-State BPMN Model Package

## Purpose

The purpose of this model package is to represent the assumed current-state FX Forward lifecycle using BPMN 2.0.

It provides a readable and traceable hierarchy of Level 1 and Level 2 process models showing process stages, sequence flows, stakeholder responsibilities, material data inputs and outputs, successful outcomes, and exception and recovery paths.

The package does not represent the target-state process and does not include `SYS-INT` or any other proposed target-state orchestration capability.

## Source Baseline

This model package is based on `DOC-CSP-001` version 0.3, committed under Git commit `aef34bb`.

The BPMN models remain traceable to the process stages, detailed activities, roles, systems, and inputs and outputs defined in the source baseline.

## Model Hierarchy

| Model ID | Model name | Coverage |
|---|---|---|
| `CSP-BPMN-L1-001` | Manage FX Forward Current-State Lifecycle | `CSP-PR-001`-`CSP-PR-008` |
| `CSP-BPMN-L2-001` | Process New-Trade Booking | `CSP-PR-001` |
| `CSP-BPMN-L2-002` | Process Amendment and Cancellation | `CSP-PR-002` |
| `CSP-BPMN-L2-003` | Process Daily Valuation | `CSP-PR-003` |
| `CSP-BPMN-L2-004` | Prepare and Monitor Maturity and Settlement | `CSP-PR-004`-`CSP-PR-005` |
| `CSP-BPMN-L2-005` | Perform Accounting and NAV Review | `CSP-PR-006` |
| `CSP-BPMN-L2-006` | Perform Reconciliation and Reporting Controls | `CSP-PR-007` |
| `CSP-BPMN-L2-007` | Coordinate Cross-Process Exceptions | `CSP-PR-008` |

## Hierarchy Rationale

We chose a Level 1 overview supported by seven Level 2 models grouped around process areas and primary ownership boundaries.

This hierarchy reduces visual complexity while preserving end-to-end traceability and making each process area easier for reviewers to understand, validate, and maintain.

## Modelling Conventions

### Process Scope and Decomposition

- The model package represents the current-state process only.
- `SYS-INT`, target-state APIs, canonical messages, automated orchestration, and automated retry capabilities are excluded.
- The Level 1 model presents the end-to-end lifecycle and links to the applicable Level 2 process models.
- Level 2 models contain the detailed operational activities, controls, decisions, hand-offs, and exception paths.
- A Level 3 model should be introduced only where a Level 2 model cannot remain readable without further decomposition.
- Local completion of an individual process stage does not by itself represent successful end-to-end lifecycle completion.

### Pools and Lanes

| BPMN element | Convention |
|---|---|
| Organisation pool | Each model contains one main white-box pool named `FX Forward Operations - Current State`. |
| Internal stakeholder lane | A lane is included only where the stakeholder performs an activity in the model. Stakeholders who are only informed are not represented through empty lanes. |
| System lane | A system may have a separate lane where it performs an automated current-state activity. |
| External participant pool | A counterparty, payment operator, or other external participant is represented through a separate black-box pool when communication with that participant is shown. |
| Application Support lane | Application Support is included only in the process paths involving technical investigation or recovery. |

### Flow and Data Conventions

| BPMN element | Convention |
|---|---|
| Sequence flow | Represents the order of activities within the same pool, including flows crossing internal lanes. |
| Message flow | Represents communication between separate pools and must not be used between lanes belonging to the same pool. |
| Data association | Connects an activity with the data object or data store it produces, consumes, or updates. It does not represent process execution order. |
| Data object | Represents a transient file, report, instruction, or processing result used during the process. |
| Data store | Represents persistent or maintained information, such as the FAP accounting state, operational lifecycle tracker, shared spreadsheet, or reconciliation records. |

Material inputs and outputs defined as `CSP-IO-001` through `CSP-IO-008` must be associated with at least one activity that produces, consumes, or updates them. Detailed data fields remain documented in `DOC-CSP-001` or the BPMN element description and are not repeated on the diagram where they would reduce readability.

### Activity Types

| Activity type | Usage |
|---|---|
| Service task | An activity performed automatically by a current-state system. |
| User task | An activity performed by a user through a system or controlled workflow. |
| Manual task | A control, transformation, investigation, or coordination activity performed outside a controlled system workflow. |
| Send or receive task | An explicit communication with a participant represented by a separate pool. |

A business rule task is not used where an operational employee manually interprets or assesses the applicable rule.

### Gateways and Events

- An exclusive gateway is used where only one alternative outcome can be selected.
- A parallel gateway is used where independent activities or paths must proceed and subsequently synchronise.
- An event-based gateway is used where the next path depends on which lifecycle event occurs.
- Outgoing gateway flows must contain conditions or otherwise identify the default path.
- Activity names use a verb and an object.
- Gateway names are expressed as questions.
- Event names describe a state or outcome rather than an activity.

Where applicable, terminal outcomes use the following names:

- `Lifecycle completed successfully`;
- `Cancellation processed successfully`;
- `Documented unsuccessful exception outcome`.

### Exception and Recovery Modelling

- The successful lifecycle path must be visually distinguishable from exception and recovery paths.
- An exception begins in the process model in which the failure or discrepancy is identified.
- An exception requiring coordination across teams is linked to `CSP-BPMN-L2-007`.
- A successfully completed recovery action may return the process to the relevant operational activity.
- An unresolved exception may terminate the process with an assigned owner, recorded status, and documented investigation, escalation, or recovery action.
- A terminal exception is a documented process outcome but is not successful lifecycle completion.
- A successfully processed cancellation is a valid lifecycle outcome and is not itself an exception. Failure to process or coordinate the cancellation may create an exception.

### Layout and Readability

- Models are arranged primarily from left to right.
- The main successful path remains on the central process axis.
- Exception and recovery paths are positioned below the main path where practicable.
- Flow crossings are minimised.
- External participant pools are positioned separately from the main organisation pool.
- Colour may support readability but must not be the only means of communicating BPMN meaning.
- Detailed identifiers and explanatory information are stored in the BPMN element description rather than added to activity labels where they would overload the diagram.

### Traceability and Validation

- Every model has a unique model ID and descriptive name.
- Every Level 1 process stage is mapped to the applicable Level 2 model and `CSP-PR` identifier.
- Level 2 element descriptions identify the applicable detailed activity IDs, such as `CSP-NT`, `CSP-LC`, `CSP-VL`, `CSP-ST`, `CSP-AC`, or `CSP-RC`.
- Relevant `CSP-IO` identifiers are recorded for activities that produce or consume material process information.
- The model package README maintains the relationship between models, process stages, detailed activities, inputs and outputs, and performers.
- The models must pass the available ADONIS:CE BPMN validation without unresolved modelling errors before publication.

## Repository and File Conventions

The current-state BPMN package uses the following planned repository structure:

```text
current-state/
├── README.md
├── source/
|   └── BPMN DI source files
└── exports/
    ├── pdf/
    |   └── reviewable PDF exports
    └── svg/
        └── optional scalable previews
```

The directories for model files are created when the first applicable export is available.

## Publication Formats

| Format | Requirement | Purpose |
|---|---|---|
| `.bpmn` | Mandatory | Editable and exchangeable BPMN DI 2.0 source exported from ADONIS:CE |
| `.pdf` | Mandatory | Stable review version available without access to ADONIS |
| `.svg` | Optional | Scalable model preview suitable for repository navigation and documentation |

The `.bpmn`, `.pdf`, and optional `.svg` files representing the same model use an identical base filename.

Exported source files are not edited manually without subsequently importing and validating the model in a BPMN-compatible modelling tool. Review exports are regenerated after an accepted change to the corresponding model.

## File Naming

- Filenames use lowercase kebab-case.
- Every filename begins with the lowercase representation of the applicable model ID.
- Dates and version numbers are not included in filenames because model history is maintained through Git.
- The same filename base is retained across all publication formats.

## Model File Inventory

| Model ID | Filename base | Status |
|---|---|---|
| `CSP-BPMN-L1-001` | `csp-bpmn-l1-001-manage-fx-forward-current-state-lifecycle` | Draft |
| `CSP-BPMN-L2-001` | `csp-bpmn-l2-001-process-new-trade-booking` | Draft |
| `CSP-BPMN-L2-002` | `csp-bpmn-l2-002-process-amendment-and-cancellation` | Draft |
| `CSP-BPMN-L2-003` | `csp-bpmn-l2-003-process-daily-valuation` | Planned |
| `CSP-BPMN-L2-004` | `csp-bpmn-l2-004-prepare-and-monitor-maturity-and-settlement` | Planned |
| `CSP-BPMN-L2-005` | `csp-bpmn-l2-005-perform-accounting-and-nav-review` | Planned |
| `CSP-BPMN-L2-006` | `csp-bpmn-l2-006-perform-reconciliation-and-reporting-controls` | Planned |
| `CSP-BPMN-L2-007` | `csp-bpmn-l2-007-coordinate-cross-process-exceptions` | Planned |
