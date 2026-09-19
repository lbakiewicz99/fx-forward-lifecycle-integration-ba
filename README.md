# FX Forward Lifecycle Integration - Business Analysis Case Study

A documentation-led Business Analysis and Business System Analysis portfolio project covering end-to-end lifecycle of deliverable FX Forwards in a fictional invesmtent organisation.

The projet demonstrates how an integration problem can be developed from a business case and scope boundary into traceable process models, requirements, data contracts, controls, test scenarios, and a small runnable implementation.

> **Portfolio notice:** This repository is a fictional case study. It does not represent a real organisation, production implementation, employer process, proprietary system, stakeholder approval, or real transaction data.

## Current Status

| Area | Status |
|---|---|
| Current phase | Current-state analysis and BPMN modelling |
| Analysis baseline | Cusiness case, scope, stakeholder analysis, and current-state process documented |
| BPMN package | Level 1 lifecycle model and 3 of 7 planned Level 2 models drafte |
| Latest increment | `CSP-BPMN-L2-003` - Process Daily Valuation |
| Next planned model | `CSP-BPMN-L2-004` - Prepare and Monitor Maturity and Settlement |
| Delivery tracking | Private Jira backlog with selected public evidence and Git commit traceability |

The BPMN artefacts remain in **Draft** status pending completion and review of the full current-state model package.

## Business Problem

The fictional organisation processes FX Forward lifecycle information through a combination of manual input, batch files, operational chcks, spreadsheets, and independently designed point-to-point hand-offs.

This creates a risk that new trades, amendments, cancellations, valuations, maturity events, settlemetn outcomes, and accounting effects are processed late or represented differently across systems. Operations may also lack a single traceable view of which trade version was accepted, rejected, corrected, or completed by each participant.

The proposed direction is a controlled canonical Integration Layer that validates supported lifecycle events, preserves trade and event identity, distributes consistent information, records, processing outcomes, and supports standard exception and recovery behaviour.

## What This Project Demonstrates

- translating an operational problem into business objectives and measurable outcomes;
- defining product, lifecycle, system, data, and implementation boundaries;
- separating facts, assumptions, open questions, observed conditions, and root-cause hypotheses;
- stakeholder analysis, decision-right definition, and engagement planning;
- current-state process discovery and hierarchical BPMN 2.0 modelling;
- modelling happy paths, controls, exceptions, recovery loops, and terminal outcomes;
- maintaining traceability between process stages, detailed activities, inputs, outputs, systems, and owners;
- incremental delivery through Jira-managed work items and version-controlled repository evidence.

## Lifecycle and System Scope

The case study covers the supported lifecycle from an executed FX Forward being recorded through valuation, maturity, settlement, accounting, reconciliation, exception management, and NAV-related controls.

| System ID | Logical system | Responsibility in the case study |
|---|---|---|
| `SYS-TBS` | Trade Booking System | Source of trade economics and lifecycle instructions |
| `SYS-INT` | Integration Layer | Proposed target-state validation, orchestration, status, and audit capability |
| `SYS-RDS` | Reference Data Service | Reference-data validation inputs |
| `SYS-VAL` | Valuation Service | Daily valuation results |
| `SYS-FAP` | Fund Accounting Platform | Accounting book of record |
| `SYS-SET` | Settlement/Cash Gateway | Settement instruction and status processing |
| `SYS-REC` | Reconciliation Platform | Reconciliation and control results |
| `SYS-NAV` | NAV and Reporting Layer | Downstream NAV and reporting consumption |

The current-state BPMN package intentionally excludes `SYS-INT` and other target-state orchestration capabilities.

## Artefact Navigator

| Artefact | Purpose | Status |
|---|---|---|
| [Business Case](docs/01-project-context/01-business-case.md) | Defines the problem, objectives, options, expected benefits, and recommended capability | Draft |
| [Scope](docs/01-project-context/02-scope.md) | Established authoritative product, process, system, data, and implementation boundaries | Draft |
| [Stakeholder Analysis](docs/01-project-context/03-stakeholder-analysis.md) | Defines stakeholder interests, influence, decision rights, and engagement approach | Draft |
| [Current-State Process](docs/02-process-and-architecture/01-current-state-process.md) | Documents the assumed end-to-end process, controls, pain points, and validation  questions | Working draft |
| [Current-State BPMN Package](docs/02-process-and-architecture/diagrams/current-state/README.md) | Defines the model hierarchy, conventions, validation rules, and file inventory | In progress |
| [Delivery and Jira evidence](docs/06-delivery/README.md) | Shows the delivery approach, Definition of Done, and issue-to-commit traceability | In progress |

## Current-State BPMN Progress

| Model ID | Model | Coverage | Status |
|---|---|---|---|
| `CSP-BPMN-L1-001` | Manage FX Forward Current-State Lifecycle | `CSP-PR-001` - `CSP-PR-008` | Draft |
| `CSP-BPMN-L2-001` | Process New-Trade Booking | `CSP-PR-001` | Draft |
| `CSP-BPMN-L2-002` | Process Amendment and Cancellation | `CSP-PR-002` | Draft |
| `CSP-BPMN-L2-003` | Process Daily Valuation | `CSP-PR-003` | Draft |
| `CSP-BPMN-L2-004` | Prepare and Monitor Maturity and Settlement | `CSP-PR-004` - `CSP-PR-005` | Planned |
| `CSP-BPMN-L2-005` | Perform Accounting and NAV Review | `CSP-PR-006` | Planned | 
| `CSP-BPMN-L2-006` | Perform Reconciliation and Reporting Controls | `CSP-PR-007` | Planned |
| `CSP-BPMN-L2-007` | Coordinate Cross-Process Exceptions | `CSP-PR-008` | Planned |

Editable BPMN 2.0 sources are maintained under [`docs/02-process-and-architecture/diagrams/current-state/source`](docs/02-process-and-architecture/diagrams/current-state/source/).

## Suggested Review Path

For a concise revew of this project:

1. Read the [Business Case](docs/01-project-context/01-business-case.md) for the business problem and recommended direction.
2. Review the [Scope](docs/01-project-context/02-scope.md) for the supported lifecycle, boundaries, and completion criteria.
3. Use the [Stakeholder Analysis](docs/01-project-context/03-stakeholder-analysis.md) to understand ownership and decision rights.
4. Follow the [Current-State Process](docs/02-process-and-architecture/01-current-state-process.md) from process assumptions through pain points and validation questions.
5. Open the [BPMN package index](docs/02-process-and-architecture/diagrams/current-state/README.md) for model hierarchy and modelling conventions.
6. Review [Delivery and Jira Evidence](docs/06-delivery/README.md) for incremental delivery and repository traceability.

## Delivery approach

The project is delivered incrementally using Jira for backlog and sprint management, Confluence for selected working documentation, ADONIS:CE for BPMN modelling, and GitHub for versioned delivery artefacts.

Jira remains private. Selected sanitised screenshots and traceability records are published in the repository so that delivery evidence can be reviewed without exposing the underlying Jira workspace.

Repository commits for newer delivery items follow the convention:

```text
FXF-<issue-number>: <imperative delivery summary>
```
## Roadmap

- [X] Business case and option assessment
- [X] Scope and portfolio completion criteria
- [X] Stakeholder analysis and governance model
- [X] Current-state process analysis
- [ ] Complete the remaining current-state Level 2 BPMN models
- [ ] Define the target-state process and system interactions
- [ ] Produce requirements, business rules, and acceptance criteria
- [ ] Define canonical data, mappings, interfaces, and error behaviour.
- [ ] Produce UAT, control, and traceability artefacts
- [ ] Implement and test the selected FastAPI and SQL evidence

## Repository Structure

```text
docs/
├── 01-project-context/
├── 02-process-and-architecture/
├── 06-delivery/
```

Additional directories will be added as reviewed requirements, data, integration, testing, and control artefacts become available. Empty placeholders are not treated as delivery progress.

## Tools and Techniques

`Business analysis` · `Business system analysis` · `FX Forwards` · `Fund accounting` · `BPMN 2.0` · `ADONIS:CE` · `Jira` · `Confluence` · `Git` · `Markdown` · `API analysis` · `OpenAPI` · `SQL` · `Python` · `FastAPI` · `pytest`

## Limitations

All organisations, systems, responsibilities, controls, data, and scenarios in this repository are fictional or generic. The analysis demonstrates a structured approach but does not claim stakeholder validation, regulatory approval, production readiness, or implementation within a real financial institutions