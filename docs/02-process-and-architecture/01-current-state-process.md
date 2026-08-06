# FX Forward Trade Lifecycle Integration - Current-State Process

## Document Control

| Field | Value |
|---|---|
| Document ID | `DOC-CSP-001` |
| Project | FX Forward Trade Lifecycle Integration BA Case Study |
| Repository | `fx-forward-lifecycle-integration-ba` |
| Version | 0.1 |
| Status | Working Draft |
| Date | 2026-08-06 |
| Document Owner | Business Analysis |
| Related documents | `DOC-BC-001` - Business Case; `DOC-SCP-001` - Scope; `DOC-STK-001` - Stakeholder Analysis |
| Classification | Public portfolio artefact - fictional scenario and synthetic data |

### Version History

| Version | Date | Change | Author |
|---|---|---|---|
| 0.1 | 2026-07-29 | Initial current-state process analysis | Portfolio author |
| 0.2 | 2026-08-06 | Corrected document structure and language; refined the current-state analysis foundation and process boundaries | Portfolio author |

## 1. Purpose

This document describes the assumed current-state process for handling deliverable FX Forward lifecycle events across trade booking, fund accounting, valuation, settlement, reconciliation, and NAV-related operations.

Its purpose is to:

- establish an explicit process baseline before defining the target state;
- describe current manual activities, system hand-offs, operational controls, and exception-recovery paths;
- identify where responsibilities and processing outcomes move between stakeholder groups;
- distinguish local system acceptance from end-to-end lifecycle completion;
- identify current-state pain points, control gaps, and root-cause hypotheses;
- provide an analysis baseline for process, requirement, interface, data-mapping, control and test artefacts produced later in the project.

This document does not design the target solution. Potential improvements identified during the analysis are recorded separately as improvement candidates and do not form part of the current-state process.

## 2. Case-Study Status and Evidence Basis

This is a fictional portfolio case study. It does not describe a real organisation, employer process, production system, operational incident, or implemented solution.

The current state has been constructed as a realistic analysis baseline using generic financial-services operating practices and explicitly documented case-study assumptions. It has not been validated through interviews with real stakeholders or inspection of production evidence.

In a real delivery, the current-state analysis would require evidence such as:

- interviews and process walkthroughs with operational teams and system owners;
- standard operating procedures and control descriptions;
- sample source extracts, import templates, and processing reports;
- system screenshots, status definitions, and interface documentation;
- exception logs, incident records, and operational metrics;
- reconciliation reports and NAV control evidence;
- cut-off calendars, service-level agreements, and escalation procedures;
- observation of representative new trade, amendment, cancellation, valuation, maturity, and settlement scenarios.

The following classification is used throughout this document:

| Classification | Meaning |
|---|---|
| Established project fact | A boundary or design fact already established by the approved project context and scope artefacts |
| Current-state assumption | A plausible AS-IS behaviour adopted to enable the fictional case study and requiring validation in a real organisation |
| Observed condition | A current-state condition derived from the adopted process assumptions |
| Root-cause hypothesis | A possible explanation for an observed condition that would require further investigation and evidence |
| Improvement candidate | A potential future-state change retained outside the current-state process |
| Open question | An unresolved matter requiring stakeholder confirmation in a real delivery |

Acceptance of a current-state assumption means that it is suitable for modeling the fictional baseline. It does not mean that the process is considered efficient, controlled, or appropriate for the target state.

## 3. Relationship to other Artefacts

| Artefact | Relationship to this document |
|---|---|
| `DOC-BC-001` - Business Case | Established why lifecycle integration is needed and identifies the assumed business problem |
| `DOC-SCP-001` - Scope | Defines the authoritative product, lifecycle, system, implementation, and reporting boundaries |
| `DOC-STK-001` - Stakeholder Analysis | Defines the stakeholder groups, specialist inputs, concerns, and decision-right boundaries used in the process analysis |
| `DOC-CSP-001` - Current-State Process | Describes how the assumed process operates before the proposed integration capability is introduced |
| Target-State Process | Will define the future process response to the validated current-state problems without rewriting the AS-IS baseline |
| Requirements and interface artefacts | Will translate approved target-state needs into testable system behaviour and data contracts |

Where this document identifies a possible improvement, the improvement must be assessed separately against the approved scope before becoming a target-state requirement.

## 4. Analysis Approach

The current-state process is analysed from two complementary perspectives.

### 4.1 Business Perspective

The business analysis considers:

- process triggers and expected outcomes;
- operational activities and hand-offs;
- process and exception ownership;
- manual review and intervention points;
- operational cut-offs and escalation dependencies;
- controls performed before accounting, settlement, reconciliation, and NAV-related activities;
- recovery actions for rejected or incomplete processing;
- business consequences of delayed, incorrect, duplicated, or missing events.

### 4.2 System Perspective

The system analysis considers:

- systems participating in each process stage;
- file-based and manual interfaces;
- source and downstream identifiers;
- file-level and record-level processing outcomes;
- lifecycle-event and trade-version handling;
- data transformations and reference-data dependencies;
- visibility of acknowledgements, rejection reasons, and processing statuses;
- dependencies between trade, valuation, accounting, settlement, and reconciliation records;
- preservation of historical trade versions and processing evidence.

### 4.3 Analysis Discipline

The analysis separates:

1. the current-state activity or condition;
2. the resulting business or operational impact;
3. the control currently used to reduce that impact;
4. the weakness remaining after the control;
5. the possible root cause;
6. any future improvement idea.

A proposed solution is not treated as evidence of the root cause. For example, the absence of an Integration Layer may describe the current architecture or suggest a future solution, but the underlying root-cause hypothesis may instead be independently designed interfaces, inconsistent data contracts, or fragmented process ownership.

## 5. Current-State Process Boundary

### 5.1 Start Boundary

The current-state process begins after an FX Forward has been executed and recorded in `SYS-TBS`.

The first current-state operational hand-off occurs when `SYS-TBS` generates a scheduled trade extract containing a new trade, amendment, or cancellation event and makes the extract available for downstream processing.

This differs from the target-state start event defined in `DOC-SCP-001`, where `SYS-TBS` will submit the lifecycle event to `SYS-INT`. `SYS-INT` does not exist as a central orchestration component in the assumed current state.

### 5.2 End Boundary

The current-state process reaches a terminal operational outcome when the relevant lifecycle activity has either:

1. completed its required fund-accounting, valuation, settlement, reconciliation, and NAV-related controls; or
2. produced an identified exception with an assigned owner, recorded status, and pending or completed recovery action.

An exception is a valid documented current-state outcome, but it is not treated as successful lifecycle completion.

For a trade reaching its contractual value date, successful completion requires:

- the applicable maturity and accounting activities to be recorded;
- both currency legs to have known final or recovery statuses;
- the relevant reconciliation controls to have been performed;
- any material NAV-impacting exception to have been reviewed or escalated.

### 5.3 Excluded Activities

The current-state analysis does not describe:

- trade idea generation or investment decision-making;
- trade execution, negotiation, confirmation, or affirmation;
- market-data sourcing or valuation-model calculations;
- reference-data creation and maintenance procedures in detail;
- internal fund-accounting ledger logic;
- execution of external payments;
- NAV calculation or publication logic;
- production incident-management procedures;
- target-state APIs, canonical messages, automated orchestration, or retry design.

These exclusions are inherited from `DOC-SCP-001`.

## 6. Established Project Facts

The following facts are inherited from the approved project context and scope:

- the supported product is a bilateral, physically settled, deliverable FX Forward;
- each trade contains two currency legs viewed from the fund's perspective;
- `SYS-TBS` is the authoritative source for trade economics and source lifecycle instructions;
- `SYS-VAL` is the authoritative source for calculated valuation results;
- `SYS-FAP` is the accounting book of record;
- `SYS-SET` is the authoritative source for the processing status of each settlement instruction;
- `SYS-REC` performs reconciliation and control activities using data obtained independently from participating systems;
- `SYS-NAV` consumes accounting and control information but its internal NAV-calculation logic is outside scope;
- the contractual value date acts as the maturity trigger for the core deliverable FX Forward scenario;
- actual settlement outcomes are recorded independently for each currency leg;
- historical trade versions, valuation records, accounting entries, and settlement evidence must remain available;
- the target-state `SYS-INT` capability is not part of the assumed current-state architecture.

All detailed process behaviour introduced after this section is classified as a current-state case-study assumption unless explicitly identified otherwise.
