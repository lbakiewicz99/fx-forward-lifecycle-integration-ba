# FX Forward Trade Lifecycle Integration - Current-State Process

## Document Control

| Field | Value |
|---|---|
| Document ID | `DOC-CSP-001` |
| Project | FX Forward Trade Lifecycle Integration BA Case Study |
| Repository | `fx-forward-lifecycle-integration-ba` |
| Version | 0.3 |
| Status | Working Draft |
| Date | 2026-08-08 |
| Document Owner | Business Analysis |
| Related documents | `DOC-BC-001` - Business Case; `DOC-SCP-001` - Scope; `DOC-STK-001` - Stakeholder Analysis |
| Classification | Public portfolio artefact - fictional scenario and synthetic data |

### Version History

| Version | Date | Change | Author |
|---|---|---|---|
| 0.1 | 2026-07-29 | Initial current-state process analysis | Portfolio author |
| 0.2 | 2026-08-06 | Corrected document structure and language; refined the current-state analysis foundation and process boundaries | Portfolio author |
| 0.3 | 2026-08-08 | Consolidated end-to-end current-state analysis and validation register | Portfolio author |

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

Acceptance of a current-state assumption means that it is suitable for modelling the fictional baseline. It does not mean that the process is considered efficient, controlled, or appropriate for the target state.

## 3. Relationship to Other Artefacts

| Artefact | Relationship to this document |
|---|---|
| `DOC-BC-001` - Business Case | Establishes why lifecycle integration is needed and identifies the assumed business problem |
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
- both currency legs to be confirmed as settled;
- the relevant reconciliation controls to have been performed;
- no unresolved material exception to prevent the applicable NAV-related completion.

If one or both legs do not settle successfully, the process may instead reach a documented exception outcome once both leg statuses, the exception owner, and any required recovery action are known. That outcome remains unsuccessful until the recovery process is completed and the applicable controls confirm resolution.

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

## 7. Current-State Systems and Supporting Artefacts

| System or artefact | Current-state responsibility | Main input | Main output | Authoritative for |
|---|---|---|---|---|
| `SYS-TBS` | Records executed FX Forward trade economics and source lifecycle instructions and produces the scheduled downstream trade extract. | Executed trades and lifecycle instructions already recorded in `SYS-TBS`; their upstream creation is outside scope. | Scheduled extract containing new-trade, amendment, and cancellation events. | Trade economics and source lifecycle instructions. |
| `SYS-VAL` | Calculates valuations for FX Forward positions that it considers open and produces the scheduled valuation file. | Current trade population and applicable valuation inputs; market-data sourcing and model calculations are outside scope. | Valuation records and exceptions for positions that could not be valued. | Calculated valuation results. |
| `SYS-FAP` | Records accepted lifecycle and valuation data, maintains accounting-position state, creates accounting impacts, and produces processing and maturity outputs. | Prepared trade-lifecycle and valuation import files. | File-level and record-level processing results, booking state, accounting outputs, and maturity population. | Accounting books, postings, and accounting-position state. |
| `SYS-SET` | Processes settlement instructions separately for each currency leg and reports their processing outcomes. | Validated settlement instructions linked to the originating trade and version. | Instruction-level statuses, rejection reasons, settlement confirmations, and recovery-related statuses. | Processing status of each settlement instruction. |
| `SYS-REC` | Performs reconciliation and control activities using independent extracts from participating systems. | Scheduled or manually initiated extracts from trade, valuation, accounting, settlement, and external sources. | Matched, unmatched, missing, and timing-difference results with recorded break statuses. | Reconciliation results and recorded break status. |
| `SYS-NAV` | Consumes accounting and control information used in NAV review and release activities. | Accounting results, control evidence, and material exception information. | NAV control and release status. | NAV output and release status; internal NAV-calculation logic remains outside scope. |
| Operational lifecycle tracker | Provides a manually maintained operational view of trade-stage progress, processing outcomes, ownership, and exceptions. | Processing results and manual status updates from participating teams. | Current operational status, assigned owner, and follow-up information. | Not authoritative; authoritative data remains in the relevant source system. |
| Shared mailboxes and operational spreadsheets | Support notifications, coordination, evidence collection, and local exception tracking. | Reports, messages, extracts, investigation notes, and attachments. | Notifications, investigation evidence, local control records, and recovery updates. | Not authoritative; authoritative data remains in the relevant source system. |

`SYS-INT` is intentionally omitted because it is not present in the assumed current-state process.

## 8. Current-State Roles

| Stakeholder | Current-state responsibility | Ownership boundary |
|---|---|---|
| `STK-002` - TBS Product Owner | Monitors source processing and scheduled `SYS-TBS` extract availability and supports investigation of source-event or source-data issues. | Owns clarification of source lifecycle instructions and `SYS-TBS` behaviour, but not downstream FAP booking or settlement decisions. |
| `STK-003` - Trade Operations | Performs lifecycle validation, prepares and uploads FAP trade files, verifies resulting booking state, maintains the operational tracker, and coordinates trade-level exceptions. | Owns operational trade-lifecycle processing and local FAP booking outcomes, but not valuation calculation, settlement execution, or accounting policy. |
| `STK-004` - Settlement Operations | Performs pre-settlement controls, submits or validates leg-level settlement instructions, monitors both legs, and coordinates cancellation, recall, or cash recovery with relevant external parties. | Owns settlement-instruction processing and recovery; it does not change authoritative trade economics or accounting policy. |
| `STK-005` - Valuation Operations | Controls the daily valuation population, prepares valuation data for FAP ingestion, monitors processing results, and investigates missing, stale, duplicate, or rejected valuations. | Owns operational valuation processing and completeness controls; valuation model design and market-data sourcing remain outside scope. |
| `STK-006` - Fund Accounting and NAV Operations | Reviews valuation and accounting impacts, prepares or approves controlled adjustments, assesses material exceptions, and completes the applicable NAV-related review. | Owns accounting and NAV control decisions within scope; it does not own source lifecycle instructions or settlement execution. |
| `STK-007` - Reconciliation | Performs independent matching, classifies breaks, assigns or escalates them to relevant owners, and records reconciliation status. | Owns identification, classification, and tracking of breaks, but not correction of the originating trade, valuation, accounting, or settlement record. |
| `STK-009` - Data and Taxonomy Owner | Maintains approved data definitions, mappings, and reference rules used across project and operational artefacts. | Owns governed definitions and mapping decisions, but not day-to-day processing of individual trades. |
| Application Support | Investigates technical failures, reviews system logs, supports platform availability, and assists users where an issue cannot be resolved through an established business procedure. | Owns technical diagnosis and system support; business acceptance, accounting treatment, and exception closure remain with the relevant business owner. |

## 9. New-Trade Inputs and Outputs

| ID | Artefact | Producer | Consumer | Key information | Purpose |
|---|---|---|---|---|---|
| `CSP-IO-001` | Scheduled TBS trade extract | `SYS-TBS` | `STK-003` - Trade Operations | Source file, batch and record identifiers; `trade_id`; `trade_version`; event type, trade economics, processing timestamp. | Provides the source lifecycle population for downstream review and FAP preparation. |
| `CSP-IO-002` | Approved FAP import template | `STK-006` - Fund Accounting and NAV Operations | `STK-003` - Trade Operations | Required columns, field formats, control fields, template version, and submission conventions. | Defines the approved structure for the current-state FAP upload. |
| `CSP-IO-003` | Approved mapping and reference rules | `STK-009` - Data and Taxonomy Owner | `STK-003` - Trade Operations | Source-to-target mappings, permitted values, account and portfolio references, transformation rules, and effective dates. | Supports consistent transformation from the TBS extract to FAP-compatible values. |
| `CSP-IO-004` | Prepared FAP import file | `STK-003` - Trade Operations | `SYS-FAP` | Accepted source identifiers, trade version, event type, mapped trade economics, template version, and control totals. | Submits controlled new-trade records for FAP validation and processing. |
| `CSP-IO-005` | FAP processing results | `SYS-FAP` | `STK-003` - Trade Operations | File result, record result, accepted and rejected records, available error code and description, processing timestamp, and FAP reference. | Allows Trade Operations to reconcile the submitted population and route rejected records. |
| `CSP-IO-006` | Confirmed FAP booking state | `SYS-FAP` | `STK-003` - Trade Operations | `trade_id`, accepted version, event type, active or historical state, accounting-position state, and FAP reference. | Confirms that an accepted result produced the expected FAP booking state. |
| `CSP-IO-007` | Operational lifecycle tracker update | `STK-003` - Trade Operations | Relevant operational stakeholders | Trade and version, lifecycle stage, local processing outcome, exception status, owner, and next action. | Maintains the current operational view used to coordinate lifecycle activity. |
| `CSP-IO-008` | Exception notification and investigation evidence | `STK-003` - Trade Operations | Assigned exception owner | Source identifiers, failed control or result, available error details, investigation evidence, owner, status, and required recovery action. | Provides traceable evidence for investigation, escalation, and recovery. |

## 10. New-Trade Current-State Flow

| ID | Performer | Activity | Input | Output or decision |
|---|---|---|---|---|
| `CSP-NT-001` | `SYS-TBS` | Generate and make the scheduled trade extract available. | Executed lifecycle instructions recorded in `SYS-TBS` | `CSP-IO-001` |
| `CSP-NT-002` | `STK-003` - Trade Operations | Confirm receipt and perform file-level completeness and control checks. | `CSP-IO-001` | File accepted for record-level processing, or file-level exception recorded in `CSP-IO-008` |
| `CSP-NT-003` | `STK-003` - Trade Operations | Confirm that the file structure and source control information support record-level processing. | `CSP-IO-001` | Proceed to record review, or stop and route the affected file for investigation. |
| `CSP-NT-004` | `STK-003` - Trade Operations | Perform record-level validation of mandatory fields, formats, permitted values, and internally consistent trade economics. | `CSP-IO-001`; `CSP-IO-003` | Valid and invalid record populations. |
| `CSP-NT-005` | `STK-003` - Trade Operations | Classify each record as valid or invalid for further processing. | Record-level validation results | Valid records proceed; invalid records are documented in `CSP-IO-008` and routed to the appropriate owner. |
| `CSP-NT-006` | `STK-003` - Trade Operations | Verify trade identity, lifecycle version, event type, duplication risk, and current FAP state. | Valid source records; current `SYS-FAP` state | For the standard path: `trade_version = 1`, `event_type = NEW`, and no conflicting existing position; otherwise route an exception. |
| `CSP-NT-007` | `STK-003` - Trade Operations | Validate the applicable FAP template and mapping references and transform accepted records. | `CSP-IO-001`; `CSP-IO-002`; `CSP-IO-003` | Transformed records ready for import-file preparation. |
| `CSP-NT-008` | `STK-003` - Trade Operations | Prepare and control the FAP import file. | Transformed accepted records | `CSP-IO-004` |
| `CSP-NT-009` | `STK-003` - Trade Operations | Upload the prepared import file to `SYS-FAP`. | `CSP-IO-004` | Submitted file and available submission reference. |
| `CSP-NT-010` | `SYS-FAP` | Validate and process the submitted file and individual records. | `CSP-IO-004` | `CSP-IO-005` |
| `CSP-NT-011` | `STK-003` - Trade Operations | Reconcile submitted records with file-level and record-level processing results. | `CSP-IO-004`; `CSP-IO-005` | Accepted records identified; rejected or unexplained records documented in `CSP-IO-008` |
| `CSP-NT-012` | `STK-003` - Trade Operations | Verify the resulting FAP booking state for accepted records. | `CSP-IO-005`; current `SYS-FAP` state | `CSP-IO-006`, or an exception where the processing result and booking state do not agree. |
| `CSP-NT-013` | `STK-003` - Trade Operations | Update the operational tracker and complete the local booking outcome or route an exception. | `CSP-IO-006`; `CSP-IO-008`, where applicable | `CSP-IO-007` and either confirmed local booking completion or a documented exception outcome. |

The following current-state rules apply to this flow:

- a file-level failure prevents the affected file from entering normal record-level processing;
- a record-level failure does not automatically prevent valid records from proceeding when the file remains processable;
- records already accepted by `SYS-FAP` are not resubmitted with corrected rejected records;
- a successful FAP processing result is reconciled to the resulting booking state and is not treated as sufficient evidence on its own;
- an exception can be a documented terminal process outcome, but it is not a successful booking outcome.

## 11. Amendment and Cancellation Scenarios

| ID | Scenario | Current-state handling | Expected state and evidence |
|---|---|---|---|
| `CSP-LC-001` | Standard amendment | Trade Operations confirms the existing `trade_id`, the expected next `trade_version`, the currently active economic version, and the absence of a later or duplicate event. The amended record is transformed, controlled, submitted to `SYS-FAP`, and reconciled using the same principles as the new-trade flow. | The amended version becomes the latest effective economic version. The previous version remains queryable as historical audit evidence, and only one economic version remains operationally active. |
| `CSP-LC-002` | Cancellation received after valuation but before settlement instructions exist | Trade Operations confirms the latest accepted version, cancellation sequence, valuation and accounting state, settlement-instruction state, and applicable cut-offs. Valuation Operations and Fund Accounting review the reversal, supersession, or adjustment required for the accepted valuation and accounting impact. | The cancellation becomes the latest effective event, the trade lifecycle becomes `CANCELLED`, and no economic version remains active. Earlier trade, valuation, and accounting records remain visible. |
| `CSP-LC-003` | Cancellation received after settlement instructions have been created but neither leg has settled | Trade Operations places the lifecycle event under manual coordination. Settlement Operations identifies both legs, stops further processing where possible, checks each instruction independently, and initiates cancellation or recall. | Cancellation is not considered operationally complete until the outcome of both instructions is known and recorded. Original instruction identifiers and status history remain available. |
| `CSP-LC-004` | One or both settlement instructions cannot be stopped, or cash has already settled | Settlement Operations records the actual result of each original instruction and initiates a separate recall, cash-recovery, or compensating-payment process where required. Trade Operations coordinates lifecycle impact, while Fund Accounting and Reconciliation assess related postings and breaks. | A completed settlement remains `SETTLED` and is not overwritten as `CANCELLED`. The trade may be `PARTIALLY_SETTLED` with `RECOVERY_PENDING` until the separate recovery process reaches a known outcome. |

Before processing an amendment or cancellation, Trade Operations is assumed to verify at least:

- that `trade_id` refers to an existing trade;
- that the incoming version is the next expected version;
- that the prior version has the expected current state;
- that the same event has not already been processed;
- that no later version is already present;
- the current valuation, accounting, and settlement state where relevant;
- any operational or NAV-related cut-off affected by the event.

The latest event and the active economic position are separate concepts. For example, a successfully processed cancellation may be represented as:

```text
trade_version = 3
event_type = CANCEL
event_effectiveness = EFFECTIVE
trade_lifecycle_status = CANCELLED
position_status = CLOSED
```

## 12. Daily Valuation Flow

| ID | Performer | Current-state activity | Input | Output or decision |
|---|---|---|---|---|
| `CSP-VL-001` | `SYS-VAL` | Produce the scheduled daily valuation file for FX Forward positions that it considers open. | Current eligible position population and valuation inputs | Valuation file containing at least `trade_id`, `trade_version`, valuation date, valuation amount, valuation currency, and source timestamp |
| `CSP-VL-002` | `STK-005` - Valuation Operations | Confirm file-level controls and compare the valuation population with active positions in `SYS-FAP` and the operational lifecycle tracker. | Scheduled valuation file; current FAP population; operational tracker | Complete population accepted for record review, or completeness and timeliness exceptions identified |
| `CSP-VL-003` | `STK-005` - Valuation Operations | Identify missing, stale, duplicate, rejected, unknown-trade, and historical-version valuations. | Population comparison and record-level controls | Normal-process population and separately recorded exceptions requiring review |
| `CSP-VL-004` | `STK-005` - Valuation Operations | Manually transform accepted valuations into the required FAP import structure, perform local controls, and upload the file. | Accepted valuation records and approved FAP valuation template | Submitted FAP valuation file |
| `CSP-VL-005` | `SYS-FAP`; `STK-005` - Valuation Operations | Validate and process the valuation file, then reconcile submitted records with file-level and record-level results and confirm the applied state. | Submitted FAP valuation file | Accepted valuations applied; rejected or unexplained records routed as valuation exceptions. |
| `CSP-VL-006` | `STK-006` - Fund Accounting and NAV Operations | Review the resulting unrealised P&L and derivative asset or liability impact before the applicable NAV cut-off. | Applied valuations, accounting results, control evidence, and exceptions | Reviewed accounting and NAV impact, controlled adjustment requirement, or escalated material exception |

Late or missing valuations are escalated manually. A prior-day valuation is not assumed to be reused automatically. Treatment of any fallback or approved estimate would require separate confirmation under the applicable accounting and NAV procedures.

## 13. Maturity and Settlement Flow

| ID | Performer | Current-state activity | Input | Output or decision |
|---|---|---|---|---|
| `CSP-ST-001` | `SYS-FAP` | Produce a daily maturity population containing the latest eligible trade versions within the next five business days and matured positions that remain operationally incomplete. | Current FAP accounting-position state and contractual value dates | Population containing at least `trade_id`, `trade_version`, contractual value date, `business_days_to_maturity`, and `maturity_status` |
| `CSP-ST-002` | `STK-003` - Trade Operations; `STK-004` - Settlement Operations | Review the maturity population against the latest accepted trade versions, cancellations, and operational lifecycle state. | Daily maturity population; FAP state; operational tracker | Eligible settlement population, or trade-level exception requiring clarification |
| `CSP-ST-003` | `STK-004` - Settlement Operations | Validate both currency legs, amounts, currencies, contractual value date, counterparty, settlement accounts, settlement instructions, and applicable cut-offs. | Eligible maturity population and settlement reference data | Both legs approved for instruction preparation, or settlement-preparation exception |
| `CSP-ST-004` | `STK-004` - Settlement Operations | Prepare or validate the settlement input and submit it to `SYS-SET` | Controlled settlement data for both legs | Separate settlement-instruction identifier for each leg, linked to the originating `trade_id` and `trade_version` |
| `CSP-ST-005` | `SYS-SET`; `STK-004` - Settlement Operations | Process and monitor both settlement instructions independently. | Leg-level settlement instructions | Current instruction status and available failure reason for each currency leg |
| `CSP-ST-006` | `STK-004` - Settlement Operations | Determine the overall settlement outcome using the confirmed statuses of both expected legs. | Two leg-level instruction outcomes | Trade considered successfully settled only when both legs are confirmed as settled |
| `CSP-ST-007` | `STK-004` - Settlement Operations with relevant downstream teams | Manage failed, late, cancelled, recalled, or partially settled leg as manual exception and coordinate any required cash recovery | Non-successful incompatible leg-level outcome | Known final or recovery status for each leg, assigned owner, evidence, and required recovery action |

For the case-study baseline, `approaching contractual value date` means entering a five-business-day preparation window. The applicable business-day calendar is assumed and requires validation in a real delivery.

`business_days_to_maturity` is a numeric field and is kept separate from `maturity_status`:

| Condition | `business_days_to_maturity` | `maturity_status` |
|---|---|---|
| Contractual value date remains in the preparation window | Greater than `0` | `UPCOMING` |
| Contractual value date is today or has passed | `0` or less | `MATURED` |

Maturity and settlement are separate dimensions. The following canonical terms are used in this case study:

| Status | Meaning |
|---|---|
| `MATURED` | The trade has reached or passed its contractual value date. |
| `SETTLEMENT_PENDING` | Settlement instructions exist, but at least one leg has no final outcome. |
| `SETTLED` | Both expected currency legs are confirmed as settled. |
| `PARTIALLY_SETTLED` | One leg has settled while other has not reached a compatible settled outcome. |
| `RECOVERY_PENDING` | A recall, cash recovery, or compensating payment remains required. |

## 14. Accounting Reclassification and NAV Flow

| ID | Performer | Current-state activity | Input | Output or decision |
|---|---|---|---|---|
| `CSP-AC-001` | `STK-006` - Fund Accounting and NAV Operations | Review daily valuation results, unrealised P&L, and derivative asset or liability balances for unexpected signs, classifications, movements, or missing accounting impact. | FAP valuation and accounting results; operational and reconciliation evidence | Accounting result accepted for the applicable review or issue identified |
| `CSP-AC-002` | `STK-006` - Fund Accounting and NAV Operations | Assess whether an amendment, cancellation, valuation, maturity, or settlement event requires an accounting reversal, reclassification, correction, or other adjustment. | Lifecycle state, valuation state, settlement state, and current postings | No adjustment required, or controlled adjustment requirement defined |
| `CSP-AC-003` | `STK-006` - Fund Accounting and NAV Operations | Manually prepare the controlled journal or FAP adjustment input where correction or reclassification is required. | Defined adjustment requirement and supporting evidence | Prepared journal or FAP adjustment input |
| `CSP-AC-004` | Separate maker and reviewer within `STK-006` | Confirm the trade reference, amount, currency, accounts, sign, accounting date, rationale, and supporting evidence before posting. | Prepared adjustment | Approved input, or returned item requiring correction |
| `CSP-AC-005` | `SYS-FAP`; `STK-006` - Fund Accounting and NAV Operations | Post the approved adjustment, retain the historical postings, and review the result. | Approved journal or FAP adjustment input | Confirmed accounting state, or accounting-processing exception |
| `CSP-AC-006` | `STK-006` - Fund Accounting and NAV Operations | At maturity and successful settlement, confirm that the existing unrealised valuation balance is cleared or reclassified under the applicable accounting rules and that the cumulative economic result is reflected as realised P&L with the relevant cash and settlement postings. | Maturity, settlement, valuation, and accounting results | Reviewed maturity accounting and NAV impact, or escalated unresolved item |

This document does not define specific debit and credit entries, chart-of-accounts values, or detailed ledger logic. Those matters belong in the accounting business rules. Historical postings remain visible and are not overwritten by the latest event.

## 15. Reconciliation, Exceptions and Reporting Flow

| ID | Performer | Current-state activity | Input | Output or decision |
|---|---|---|---|---|
| `CSP-RC-001` | Participating systems and operations teams | Provide scheduled or manually initiated extracts to `SYS-REC` through existing file-based or point-to-point feeds. | Data from `SYS-TBS`, `SYS-VAL`, `SYS-FAP`, `SYS-SET`, and relevant external sources. |    Reconciliation populations available independently; no central orchestration is assumed |
| `CSP-RC-002` | `STK-007` - Reconciliation | Perform separate comparisons for trade lifecycle versions, daily valuations, settlement-leg statuses, and accounting or NAV-processing results. | Available reconciliation populations and control rules | Matched items and potential breaks by control domain |
| `CSP-RC-003` | `STK-007` - Reconciliation | Correlate records using available trade, version, valuation-date, leg, settlement-instruction, batch, and record identifiers. | Potentially related records from independent sources | Matched relationship, or item requiring additional manual mapping and investigation |
| `CSP-RC-004` | `STK-007` - Reconciliation | Classify breaks as missing records, duplicates, data mismatches, status mismatches, expected timing differences, valuation breaks, accounting breaks, or settlement breaks. | Unmatched or inconsistent records | Classified break, agreed resolution date where applicable, and escalation requirement |
| `CSP-RC-005` | `STK-007` - Reconciliation and assigned business owner | Assign ownership, collect evidence, monitor progress, and record the current exception outcome across available system reports, mailboxes, trackers, and spreadsheets. | Classified break and available investigation evidence | Updated break status, owner, evidence, and required recovery action |
| `CSP-RC-006` | `STK-006` - Fund Accounting and NAV Operations with relevant control owners | Complete the applicable operational controls and review material breaks before releasing NAV and related reporting outputs. | Reconciliation results, exception summaries, accounting results, and manual extracts | Release decision, approved monitored exception, or escalation; no central operational dashboard is assumed |

The absence of a shared end-to-end event or correlation identifier increases the manual effort required to connect these independent records. Known timing differences may remain under monitoring until an agreed resolution date; unclassified or overdue breaks require investigation.

## 16. Status, Versioning and Audit-Trail Model

### 16.1 Core Principles

| ID | Current-state principle |
|---|---|
| `CSP-SM-001` | A stable `trade_id` links all lifecycle events and historical versions of the same economic contract. |
| `CSP-SM-002` | Each accepted lifecycle change uses the next expected `trade_version`; duplicate, skipped, or stale versions require investigation. |
| `CSP-SM-003` | Version status, event-processing status, event effectiveness, trade lifecycle, position state, and settlement state are separate status dimensions. |
| `CSP-SM-004` | No more than one economic trade version is operationally active at a time. |
| `CSP-SM-005` | A successfully processed cancellation is the latest effective event but results in a `CANCELLED` trade lifecycle and a `CLOSED` position. |
| `CSP-SM-006` | Historical trade versions, valuations, accounting postings, settlement instructions, completed settlements, and exception evidence are retained rather than overwritten or deleted. |
| `CSP-SM-007` | Local processing success is not treated as end-to-end success until the applicable downstream state and controls have been confirmed. |

### 16.2 Status Dimensions

| Dimension | Question answered | Canonical examples |
|---|---|---|
| Version status | Is this the latest or a historical version? | `LATEST`, `HISTORICAL` |
| Event-processing status | Has the submitted event been received, validated, processed, or rejected? | `RECEIVED`, `VALIDATED`, `PROCESSED`, `REJECTED` |
| Event effectiveness | Does this event currently determine the economic lifecycle state? | `EFFECTIVE`, `SUPERSEDED` |
| Trade-lifecycle status | What is the economic lifecycle state of the trade? | `ACTIVE`, `CANCELLED`, `MATURED` |
| Position status | Does an open economic position remain? | `OPEN`, `CLOSED` |
| Settlement status | What is the combined operational outcome of the two currency legs? | `NOT_INITIATED`, `SETTLEMENT_PENDING`, `SETTLED`, `PARTIALLY_SETTLED`, `RECOVERY_PENDING` |

These status names are canonical case-study terms and do not claim to reproduce the configuration of a real FAP or settlement platform.

### 16.3 Cancellation Example

| Version | Event | Version status | Processing status | Event effectiveness | Resulting lifecycle state |
|---|---|---|---|---|---|
| 1 | `NEW` | `HISTORICAL` | `PROCESSED` | `SUPERSEDED` | Historical evidence only |
| 2 | `AMEND` | `HISTORICAL` | `PROCESSED` | `SUPERSEDED` | Historical evidence only |
| 3 | `CANCEL` | `LATEST` | `PROCESSED` | `EFFECTIVE` | `CANCELLED`; position `CLOSED` |

### 16.4 Minimum Audit Evidence

The current-state evidence is assumed to allow an investigator to establish, where applicable:

- `trade_id`, `trade_version`, and event type;
- source file, batch, and record identifiers;
- previous and resulting lifecycle, position, and processing states;
- which version is latest and which versions are historical;
- source, processing, and control timestamps;
- the user or process performing a manual or system action;
- relevant valuation and accounting events, including reversals or adjustments;
- settlement-instruction and leg identifiers and their status histories;
- the exception classification, owner, evidence, decision, and recovery action.

## 17. Current-State Pain Points and Root-Cause Hypotheses

| ID | Observed current-state condition | Business impact | Existing control | Control gap | Root-cause hypothesis | Evidence status |
|---|---|---|---|---|---|---|
| `CSP-PP-001` | Lifecycle events and processing results are tracked across system reports, shared mailboxes, and operational spreadsheets without a consistent end-to-end event identifier. | Investigation may be delayed and users may act on an outdated trade version before the relevant operational cut-off. | Operations manually compare batch, record, trade, and version identifiers across available reports. | Identifiers and statuses are not consistently propagated between systems. | Existing point-to-point interfaces and operational trackers were designed independently. | `Case-study assumption` |
| `CSP-PP-002` | Accepted valuation records are manually transformed from the `SYS-VAL` output into the import structure required by `SYS-FAP` and then manually uploaded. | Manual transformation increases processing time and may result in delayed or incorrect valuation and P&L updates before the NAV cut-off. | Valuation Operations validates the completeness, compares the population with active FAP positions, uses the approved import template, and reviews FAP import results. | The transformation and mapping are not executed or validated through a system-controlled interface. | `SYS-VAL` and `SYS-FAP` use independently defined data structures and are connected through a manual file-based process. | `Case-study assumption` |
| `CSP-PP-003` | Each currency leg is processed and reported separately, while Settlement Operations manually determines the overall settlement state of the trade. | Manual aggregation increases operational effort and creates a risk that a trade is considered settled while one leg remains failed, pending, cancelled or subject to cash recovery. | Settlement Operations compares both instruction statuses, cash-ledger movements, and settlement reports using the trade, version, leg, and settlement-instruction identifiers. | No automated trade-level completeness rule or consolidated alert confirms that both expected legs have reached compatible final statuses. | Settlement instructions are processed independently and current reporting does not orchestrate their combined lifecycle outcome. | `Case-study assumption` |
| `CSP-PP-004` | Amendment and cancellation events for previously exported trades are analysed and prepared manually before submission to `SYS-FAP` and notification of affected downstream teams. | An incomplete assessment of the current trade, valuation, accounting, or settlement state may result in processing the wrong version or preparing an incorrect downstream instruction. | Trade Operations manually verifies the trade identity, expected version, current FAP position, and downstream processing state before preparing the required instruction. | The control depends on manual interpretation and matching, without automated sequence, idempotency, or current-state validation. | Lifecycle events are not automatically correlated and orchestrated across systems using a shared event and version model. | `Case-study assumption` |
| `CSP-PP-005` | NAV and reporting outputs are released after separate operational controls based on system reports and manual extracts from multiple systems. | Fragmented control evidence may delay NAV review and obscure incomplete or inconsistent lifecycle, valuation, settlement, or accounting data before the NAV cut-off | Fund Accounting, NAV Operations, and Reconciliation review control totals, exception reports, reconciliation breaks, and supporting extracts before release. | There is no consolidated view linking processing results and exceptions to the same trade version and lifecycle event. | Control processes and system feeds were developed independently around point-to-point and file-based interfaces. | `Case-study assumption` |
| `CSP-PP-006` | File-processing and FAP import results do not always provide a standardised and actionable rejection code and description at record level. | Operations may spend additional time classifying errors, involve Application Support unnecessarily, and miss an operational or NAV cut-off. | Operations collects the available batch, record, trade, and raw error details, while Application Support reviews system logs and available documentation. | Unknown or inconsistent rejection information cannot be reliably routed to a standard recovery action and accountable owner. | Error-handling conventions were defined independently and are not governed through a shared error catalogue and interface contract. | `Case-study assumption` |

The hypotheses in this section explain potentially observable conditions and outcomes for further investigation. They are not presented as proven causes. In particular, the absence of an Integration Layer is not treated as a root cause: it may be an architectural condition or motivate a preventive improvement, while the underlying cause may relate to independently designed data contracts, status models, controls, or ownership.

## 18. Improvement Candidates

| ID | Improvement candidate | Current-state relationship | Assessment status |
|---|---|---|---|
| `CSP-IMP-001` | Automate standard trade and reference-data validation through the proposed `SYS-INT` capability and route non-standard exceptions for manual review. | Reduces routine manual validation while retaining business control over ambiguous or unsupported cases. | Candidate for target-state assessment |
| `CSP-IMP-002` | Introduce structured error codes, actionable rejection descriptions, and standard ownership and recovery guidance. | Responds to inconsistent or non-actionable file-level and record-level rejection information. | Candidate for target-state assessment |
| `CSP-IMP-003` | Replace routine manual FAP import preparation and upload with controlled transformation and submission using the approved template and mappings, preserve source identifiers, and resubmit only corrected rejected records. | Responds to manual file preparation and reduces duplicate-processing risk after partial acceptance. | Candidate for target-state assessment |
| `CSP-IMP-004` | Introduce version-aware amendment and cancellation controls, including sequence, idempotency, and current-state validation. | Responds to manual lifecycle-event correlation and assessment of affected downstream state. | Candidate for target-state assessment |
| `CSP-IMP-005` | Automate valuation file-level and record-level validation and present identified exceptions to operational users. | Responds to recurring valuation completeness, format, timeliness, version, and duplicate controls. | Candidate for target-state assessment |
| `CSP-IMP-006` | Provide a controlled accounting-adjustment loader generator with field validation and maker-checker controls. | Supports preparation of recurring accounting corrections while retaining controlled review and approval. | Candidate for target-state assessment |
| `CSP-IMP-007` | Introduce an Integration Layer that coordinates, correlates, and monitors participating system feeds. | Provides a possible architectural response to independently operated feeds and fragmented processing visibility. | Candidate for target-state assessment |
| `CSP-IMP-008` | Provide consolidated operational-control reporting for NAV outputs, material exceptions, and suspicious divergences. | Responds to fragmented control evidence and manual compilation before NAV release. | Candidate for target-state assessment |

These items are retained for target-state analysis. They are not confirmed requirements, approved solution components, or evidence that the absence of a proposed component is itself a root cause.

## 19. End-to-End Process Inventory

| ID | Process stage | Trigger or input | Primary operational owner | Participating systems | Local outcome | Detailed section |
|---|---|---|---|---|---|---|
| `CSP-PR-001` | New-trade booking | Scheduled `NEW` event in the `SYS-TBS` extract | `STK-003` - Trade Operations | `SYS-TBS`, `SYS-FAP` | Confirmed FAP booking or documented exception | Sections 9-10 |
| `CSP-PR-002` | Amendment and cancellation | Scheduled `AMEND` or `CANCEL` event for an existing `trade_id` | `STK-003` - Trade Operations | `SYS-TBS`, `SYS-FAP`, and downstream `SYS-VAL` or `SYS-SET` where applicable | Latest effective lifecycle state or documented exception and recovery requirement | Section 11 |
| `CSP-PR-003` | Daily valuation | Scheduled daily valuation file | `STK-005` - Valuation Operations | `SYS-VAL`, `SYS-FAP` | Applied valuation and accounting impact or documented valuation exception | Section 12 |
| `CSP-PR-004` | Maturity preparation | Trade enters the five-business-day maturity window | `STK-003` - Trade Operations; `STK-004` - Settlement Operations | `SYS-FAP`, `SYS-SET` | Validated maturity population and submitted settlement instructions, or documented exception | Section 13 |
| `CSP-PR-005` | Settlement monitoring and recovery | Settlement instructions have been created | `STK-004` - Settlement Operations | `SYS-SET` and downstream accounting and reconciliation processes | Both legs settled, known partial outcome, or documented recovery requirement | Sections 11 and 13 |
| `CSP-PR-006` | Accounting and NAV review | Accepted valuation or lifecycle, maturity, or settlement event | `STK-006` - Fund Accounting and NAV Operations | `SYS-FAP`, `SYS-NAV` | Reviewed accounting and NAV outcome or material exception | Section 14 |
| `CSP-PR-007` | Reconciliation and reporting | Scheduled extracts and control cycle | `STK-007`  - Reconciliation | `SYS-REC` and participating system extracts | Reconciled outcome, assigned break, or completed control review | Section 15 |
| `CSP-PR-008` | Cross-process exception coordination | Failed control, rejection, missing result, mismatch, or incomplete operational outcome | Current process owner and assigned exception owner | Relevant systems, operational tracker, shared mailboxes, and spreadsheets | Documented exception with status, owner, evidence, and recovery action | Sections 10-15 |

This inventory provides candidate Level 2 process groupings for the subsequent ADONIS:CE model. The Level 1 model will represent the full FX Forward lifecycle. A Level 2 process does not automatically require a separate diagram; diagram boundaries will be chosen according to process readability and ownership.

## 20. Validation and Open-Question Register

| ID | Matter requiring validation | Proposed validation evidence | Primary stakeholder | Impact if assumption is incorrect |
|---|---|---|---|---|
| `CSP-VQ-001` | Confirm the actual frequency, availability time, cut-off, and control totals of the scheduled `SYS-TBS` extract. | Interface schedule, sample extracts, operating procedure, and process walkthrough. | `STK-002`, `STK-003` | The documented process timing and first operational control may be inaccurate. |
| `CSP-VQ-002` | Confirm whether `SYS-FAP` can partially accept an import file and whether it returns actionable file-level and record-level results. | Import specification, sample result files, error logs, and representative rejected records. | `STK-003`, Application Support | The partial-processing and exception-recovery flow may require redesign. |
| `CSP-VQ-003` | Confirm how `SYS-FAP` stores amendments and cancellations, including version sequence, active economic state, and visibility of historical versions. | System walkthrough, status definitions, database or audit extracts, and sample lifecycle scenarios. | `STK-003`, Application Support | The assumed versioning, cancellation, and audit-trail model may not reflect the actual system behaviour. |
| `CSP-VQ-004` | Confirm the expected daily valuation population, valuation cut-off, required identifiers, and treatment of missing or stale valuations. | Valuation procedure, sample `SYS-VAL` files, completeness controls, and exception reports. | `STK-005`, `STK-006` | Valuation completeness controls and NAV-impact analysis may be incomplete. |
| `CSP-VQ-005` | Confirm the five-business-day maturity preparation window, applicable business-day calendar, and timing of settlement-instruction creation. | Maturity schedule, settlement procedure, cut-off calendar, and operational walkthrough. | `STK-003`, `STK-004` | The maturity trigger and settlement preparation timeline may need adjustment. |
| `CSP-VQ-006` | Confirm how settlement-leg identifiers are linked to `trade_id` and `trade_version`, and how partial settlement, recall, and recovery are represented. | Settlement messages, status catalogue, cash reports, and sample failed-settlement cases. | `STK-004`, Application Support | The trade-level settlement outcome and recovery flow may be modelled incorrectly. |
| `CSP-VQ-007` | Confirm the accounting treatment of valuation, cancellation, maturity, and settlement events, including maker-checker controls and the movement from unrealised to realised P&L. | Accounting policy, posting examples, adjustment templates, and control evidence. | `STK-006` | The accounting activities and NAV dependencies may be materially inaccurate. |
| `CSP-VQ-008` | Confirm reconciliation matching keys, accepted timing differences, break categories, escalation threshold, and evidence-retention requirements. | Reconciliation rules, sample reports, aged-break reports, and escalation procedures. | `STK-007` | The documented reconciliation and exception lifecycle may omit essential controls. |
| `CSP-VQ-009` | Confirm the actual ownership, location, and status taxonomy used for cross-process exception tracking. | Operational tracker, shared-mailbox examples, procedures, and stakeholder walkthroughs. | `STK-003`, `STK-004`, `STK-005`, `STK-006`, `STK-007` | The assumed terminal exception outcome may not provide sufficient ownership or traceability. |
| `CSP-VQ-010` | Confirm which unresolved exceptions can prevent NAV release and which may proceed under documented approval or materiality threshold. | NAV sign-off checklist, materiality policy, approval matrix, and historical exception cases. | `STK-006` | The process end boundary and escalation conditions may require revision. |

These matters do not prevent completion of the fictional current-state baseline. They identify where a real delivery would require stakeholder confirmation and supporting evidence before the process model could be formally approved.
