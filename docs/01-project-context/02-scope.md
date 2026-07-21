# FX Forward Trade Lifecycle Integration - Scope

## Document Control

| Field | Value |
|---|---|
| Document ID | `DOC-SCP-001` |
| Project | FX Forward Trade Lifecycle Integration BA Case Study |
| Repository | `fx-forward-lifecycle-integration-ba` |
| Version | 0.1 |
| Status | Draft for review |
| Date | 2027-07-19 |
| Document owner | Business Analysis |
| Parent document | `DOC-BC-001` - Business Case |
| Classification | Public portfolio artefact - fictional scenario and synthetic data |

### Version History

| Version | Date | Change | Author |
|---|---|---|---|
| 0.1 | 2026-07-19 | Initial scope baseline | Repository owner |   

## 1. Purpose

This document defines the authoritative scope of the FX Forward Trade Lifecycle Integration BA Case Study.

It establishes:

- the supported financial product and lifecycle boundaries;
- the business capabilities included in the analysis;
- the logical systems and interfaces within the target landscape;
- the data, control, reporting, and error-handling boundaries;
- the distinction between full analysis scope and the limited runnable implementation;
- explicit exclusions that protect the case study from uncontrolled expansion;
- the criteria that must be satisfied for the portfolio project to be considered complete.

The Business Case (`DOC-BC-001`) explains why the change is valuable. This document defines what the project will and will not cover. Detailed behaviour will be specified later through process models, requirements, business rules, interface contracts, mappings, and test cases.

> **Scope authority:** If another project artefact conflicts with this document, this document controls the scope unless an approved scope change has updated the baseline.

## 2. Scope Statement

The project will analyse, specify, and demonstrate a target-state integration capability for a deliverable FX Forward from the submission of a booked trade by the Trade Booking System through validation, downstream distribution, amendmnet or cancellatio, daily valuation, maturity, illustrative accounting reclassification, two-leg settlement monitoring, reconciliation, exception handling, and NAV-readiness reporting.

The case study will define the end-to-end business and system behaviour across generic financial-services platforms. It will include a contract-first REST API, canonical data model, source-to-target mappings, synthetic data, SQL controls, and a minimal FastAPI implementation of the Integration Layer.

The project will not implement trade execution, pricing, a production fund-accounting engine, payment execution, a full reconciliation platform, or NAV calculation.

## 3. Scope Principles

| Principle | Application to this project |
|---|---|
| One product, deep lifecycle | The project covers one deliverable FX Forward in sufficient depth rather than several products superficially. |
| Contract before code | Requirements, data definitions, validation rules, and API behaviour are agreed before the mock implementation. |
| Explicit system ownership | Each business record and processing status has a defined authoritative source. |
| Separate business and processing state | Trade lifecycle, integration processing, valuation, accounting, and settlement statuses are modelled separately. |
| Exceptions are valid outcomes | A flow may end in an explicit controlled exception rather than falsely reporting success. |
| Independent control evidence | Reconciliation must be capable of detecting an event that never reached the Integration Layer. |
| Thing executable slice | Code proves selected contracts and behaviours without pretending to be a production platform. |
| Traceability by design | Objectives, requirements, rules, mappings, interfaces, errors, controls, and tests use stable identifiers. |
| No invented production evidence | The project does not fabricate real users, approvals, volumes, incidents, savings, or deployment results. |

## 4. Identifier Prefixes Introduced by This Document

| Prefix | Purpose | Example |
|---|---|---|
| `SCP-` | In-scope capability or boundary | `SCP-001` |
| `OOS-` | Explicit out-of-scope item | `OOS-001` |
| `SYS-` | Logical system identifier | `SYS-TBS` |
| `IF-` | Logical interface identifier | `IF-001` |
| `EXIT-` | Portfolio completion criterion | `EXIT-001` |

These prefixes will be reused by downstream artefacts and included in the future automated consistency checks.

## 5. End-to-End process Boundary

### 5.1 Start Boundary

The in-scope process starts after an FX Forward has been executed and recorded in the Trade Booking System. The first in-scope event is the submission of a new trade instruction from the Trade Booking System to the Integration Layer.

Trade idea generation, investment decision-making, order execution, price negotiation, confirmation, and affirmation occur before this boudnary are not out of scope.

### 5.2 End Boundary

The process reaches an in-scope terminal outcome when either:

1. the trade has been cancelled before maturity and all required downstream processing has completed or produced an explicit exception; or
2. the trade has reached its contractual value date, required maturity and accounting events have been generated, both settlement legs have final recorded outcomes, and reconciliation has either passed or produced an explicit exception.

An unresolved exception is not treated as successful completion. It is, however, a valid and auditable processing outcome within the scope of the case study.

### 5.3 Status Dimensions

The project will not use one overloaded status to represent the entire lifecycle. At minimum, the target model will distinguish:

- business lifecycle status;
- integration processing status;
- downstream accounting status;
- valuation status by valuation date;
- settlement status for each currency leg;
- reconciliation or control status.

The exact status values and permitted transitions will be defined in the FX Forward Lifecycle Model.

## 6. Product Scope

### 6.1 Supported Product

The supported product is a billateral, physically settled, deliverable FX Forward.

For this case study, each trade has:

- one stable source trade identifier;
- one current trade version and an immutable version history;
- one fund;
- one investment account or portfolio within that fund;
- one counterparty;
- one trade date;
- one contractual value date, treated as the maturity and settlement date;
- one buy currency and buy amount;
- one sell currency and sell amount;
- one agreed forward rate;
- exactly two currency legs, viewed from the fund's perspective;
- lifecycle, processing, valuation, accounting, settlement, and control records linked through stable identifiers.

The buy and sell currencies must be different. Amounts and the agreed rate must be positive. Detailed field definitions and all validation rules will be maintained in the Canonical Data Dictionary and Validation Matrix.

### 6.2 Supported Lifecycle Events

| Lifecycle event | Scope boundary |
|---|---|
| New trade | Receive, validate, version, record, and distribute a newly booked trade |
| Amendment | Accept a complete replacement snapshot for an eligible pre-maturity trade and preserve previous versions |
| Cancellation | Cancel an eligible trade before maturity and distribute the lifecycle change |
| Daily valuation | Receive externally calculated valuation results for eligible active trades |
| Maturity | Identify an active trade reaching its contractual value date and initiate the defined maturity flow |
| Accounting reclassification | Generate and track the accounting instruction required by the agreed fictional policy |
| Settlement | Create and monitor separate outcomes for the buy and sell currency legs |
| Reconciliation | Compare independent records and identify completeness or consistency breaks |
| Exception resolution | Apply the permitted retry, corrected resubmission, manual review, or escalation path |
| Closure | Record the final business and processing outcome without deleting the audit history |

## 7. In-Scope Business and System Capabilities

| Scope ID | Capability | Boudnary | Related objectives |
|---|---|---|---|
| `SCP-001` | FX Forward identification | Assign and preserve trade, event, version, correlation, and idempotency identifiers | `OBJ-002`, `OBJ-005` |
| `SCP-002` | New trade intake | Receive a supported new-trade request from the Trade Booking System | `OBJ-001`, `OBJ-006` |
| `SCP-003` | Validation | Perform schema, required-field, reference-data, cross-field, lifecycle, and version checks | `OBJ-002`, `OBJ-003` |
| `SCP-004` | Canonical transformation | Convert accepted source data into the canonical FX Forwards representation |`OBJ-002`, `OBJ-006` |
| `SCP-005` | Idempotency and version control | Prevent duplicate business effects and reject stale or invalid lifecycle versions | `OBJ-002`, `OBJ-005` |
| `SCP-006` | Downstream distribution | Route accepted lifecycle data and record acknowledgements or failure by destination | `OBJ-001`, `OBJ-005` |
| `SCP-007` | Amendment processing | Record a new immutable trade version and distribute the accepted replacement snapshot | `OBJ-001`, `OBJ-002` |
| `SCP-008` | Cancellation processing | Validate and distribute an eligible pre-maturity cancellation | `OBJ-001`, `OBJ-002` |
| `SCP-009` | Valuation population and ingestion | Identify eligible trades and receive externally calculated daily valuations | `OBJ-001`, `OBJ-004` |
| `SCP-010` | Valuation completeness control | Identify missing, duplicate, late, rejected, or inconsistent valuations by valuation date | `OBJ-003`, `OBJ-004` |
| `SCP-011` | Maturity processing | Identify trades due on the processing date and create the required maturity events | `OBJ-001`, `OBJ-004` |
| `SCP-012` | Accounting-event generation | Generate and track illustrative valuation, maturity, reclassification, and settlement accounting instructions | `OBJ-002`, `OBJ-004` |
| `SCP-013` | Settlement-leg monitoring | Create and monitor the buy and sell settlement legs independently | `OBJ-003`, `OBJ-004` |
| `SCP-014` | Exception and recovery handling | Classify failures and apply controlled retry, corrected resubmission, manual review, or escalation | `OBJ-003`, `OBJ-005` |
| `SCP-015` | Audit trail | Record receipt, validation, transofrmation, distribution, response, retry, and outcome timestamps | `OBJ-005`, `OBJ-006` |
| `SCP-016` | Integration reconciliation | Perform completeness, version, valuation, maturity, accounting-event, and settlement controls | `OBJ-002`, `OBJ-005` |
| `SCP-017` | Operational reporting | Report processing status, unresolved exceptions, missing valuations, upcoming maturities, settlement outcomes, and NAV readiness | `OBJ-003`, `OBJ-004` |
| `SCP-018` | Test and traceability evidence | Link approved behaviour to requirements, rules, interfaces, data mappings, controls, and UAT results | `OBJ-005`, `OBJ-006` |

## 8. Logical System Boundary

| System ID | Logical system | Authoritative responsibility | Scope treatment | Runnable implementation |
|---|---|---|---|---|
| `SYS-TBS` | Trade Booking System | Trade economics, source trade status, and source trade version | Interface behaviour and synthetic source data are in scope; internal booking logic is out of scope | No |
| `SYS-INT` | Integration Layer | Integration-processing audit trail, canonical transformation, distribution status, retry status, and correlation | Full target-state analysis and selected mock implementation are in scope | No |
| `SYS-RDS` | Reference Data Service | Fund, account/portfolio, currency, counterparty, and business-calendar reference data | Required lookups and failure behaviour are in scope; reference-data maintenance is out of scope | Mocked |
| `SYS-VAL` | Valuation Service | Calculated valuation result and valuation metadata | Input/output contract and synthetic results are in scope; pricing logic is out of scope | Mocked |
| `SYS-FAP` | Fund Accounting Platform | Accounting representation, postings, and accounting-processing status | Interface contract, expected acknowledgements, and accounting-event categories are in scope; legder engine is out of scope | Mocked |
| `SYS-SET` | Settlement/Cash Gateway | Settlement instruction processing and authoritative status of each currency leg | Instruction and status contracts are in scope; payment execution is out of scope | Mocked |
| `SYS-REC` | Reconciliation Platform | Control execution, break result, and reconciliation status | Data inputs, control logic, and SQL simulations are in scope; full case-management platform is out of scope | Simulated through SQL and sample data |
| `SYS-NAV` | NAV and Reporting Layer | Published NAV outputs and operational reporting consumption | Required inbound data and readiness indicators are in scope; NAV calculation and publising engine are out of scope | No |

The Integration Layer is not the source of truth for trade economics, valuation calculations, accounting books, settlement execution, or published NAV. It owns the audit record of how information was processed through the integration boundary.

## 9. Logical Interface Boundary

| Interface ID | Producer | Consumer | Information exchanged | Planned interaction style |
|---|---|---|---|---|
| `IF-001` | `SYS-TBS` | `SYS-INT` | New, amendment, and cancellation requests | REST/JSON |
| `IF-002` | `SYS-RDS` | `SYS-INT` | Fund, portfolio/account, currency, counterparty, and calendar reference data | Read-only request/response; transport to be decided |
| `IF-003` | `SYS-INT` | `SYS-VAL` | Eligible active-trade population | Scheduled canonical JSON feed or equivalent mocked request |
| `IF-004` | `SYS-VAL` | `SYS-INT` | Daily valuation results and calculation metadata | Bulk REST/JSON |
| `IF-005` | `SYS-INT` | `SYS-FAP` | Trade lifecycle, valuation, maturity, and accounting events | Asynchronously processed canonical JSON |
| `IF-006` | `SYS-FAP` | `SYS-INT` | Acceptance, rejection, and accounting-processing status | Status callback or mocked acknowledgement |
| `IF-007` | `SYS-INT` | `SYS-SET` | Buy-leg and sell-leg settlement instructions | Asynchronously processed canonical JSON |
| `IF-008` | `SYS-SET` | `SYS-INT` | Leg-level settlement status and failure details | Status callback/JSON |
| `IF-009` | `SYS-INT` | `SYS-REC` | Integration-processing, distribution, and exception records | Scheduled control dataset |
| `IF-010` | `SYS-TBS` | `SYS-REC` | Independent source trade and lifecycle snapshot or control totals | Scheduled CSV control feed |
| `IF-011` | `SYS-VAL` | `SYS-REC` | Independent valuation output | Scheduled CSV control feed |
| `IF-012` | `SYS-FAP` | `SYS-REC` | Accounting trade version, valuation, and processing status | Scheduled CSV control feed |
| `IF-013` | `SYS-SET` | `SYS-REC` | Independent settlement-leg status | Schedulec CSV control feed |
| `IF-014` | `SYS-FAP` | `SYS-NAV` | Accounting and NAV input records | Existing downstream feed represented conceptually |
| `IF-015` | `SYS-REC` | `SYS-NAV` | Control completion, exceptions, and NAV-readiness status | Existing downstream feed represented conceptually |
| `IF-016` | Operations user or source system | `SYS-INT` | Processing-status query by correlation or trade identifier | REST/JSON |

The interface identifiers describe logical information exchanges, not real vendor endpoints. The Interface Inventory will become the authoritative source for ownership, frequency, protocol, security, data format, acknowledgements, and service levels.

The OpenAPI specification will cover interfaces owner by the mock Integration Layer. External-system APIs will be represented only to the level required to define expected requests, responses, acknowledgements, mappings, and error behaviour.

## 10. Data Scope

### 10.1 In-Scope Data Domains

| Data Domain | Purpose within the case study |
|---|---|
| FX Forward Trade | Current canonical trade economics and business lifecycle state |
| Trade Version | Immutable snapshot of accepted economics for a specific version |
| Currency Leg | Buy or sell amount, currency, direction, and settlement state |
| Lifecycle Event | New, amendment, cancellation, valuation, maturity, reclassification, settlement, or closure event |
| Processing Record | Rceipt, validation, distribution, retry, acknowledgement, and outcome history |
| Valuation | Valuation date, value, currency, calculation timestamp, source, and acceptance status |
| Accounting Event | Illustrative event category and downstream accounting-processing status |
| Settlement Instruction and Leg | Payment direction, currency, amount, due date, instruction identifier, and status |
| Exception | Category, source, severity, owner, status, timestamps, and permitted recovery action |
| Reconciliation Result | Control name, comparison date, result, break reason, and related record identifiers |
| Reference Data | Fund, portfolio/account, counterparty, currency, and business-calendar values needed for validation |

### 10.2 Data Exclusions

The following data is not required

- real client, investor, employee, or counterparty personal data;
- real bank-account or standard settlement instruction details;
- complete market-data curves, pricing inputs, or pricing-model parameters;
- a production chart of accounts or full journal-entry dataset;
- complete fund holdins, cash balances, NAV calculations, or investor reporting;
- proprietary identifiers, schemas, or extracts from real employers or commercial platforms.

All identifiers and values used in examples will be synthetic.

## 11. Validation Scope

The project will specify and test the following validation categories:

| Validation category | Included examples |
|---|---|
| Schema validation | Data type, format, required field, allowed anumeration, and payload structure |
| Reference-data validation | Known and active fund, portfolio/account, currency, and counterparty |
| Cross-field validation | Different buy and sell currencies, positive amounts and rate, and consistent leg directions |
| Data validation | Trade date, contractual value date, valuation date, business date, and eligible lifecycle timing |
| Lifecycle validation | Permitted amendment, cancellation, valuation, maturity, and settlement transitions |
| Version validation | Expected next version, stale version, duplicate version, and source-version consistency |
| Idempotency validation | Duplicate event identifier or idempotency key without duplicate business effect |
| Downstram-response validation | Valid acknowledgement, matching identifiers, supported status, and response completeness |
| Control validation | Missing population, version mismatch, duplicate valuation, incomplete maturity event, or settlement-leg break |

The exact rules, messages, severity, processing stage, and recovery action will be maintained in the Validation Matrix and Error Catalogue

## 12. Accounting Scope

This project includes analysis of the accounting-event flow required to demonstrate integration with the Fund Accounting Platform.

The fictional accounting policy may distinguish:

- daily valuation and unrealised gain or loss recognition;
- derivative asset or liability classification based on the accepted valuation;
- maturity processing and reclassification from unrealised to realised result;
- creation or recognition of settlement receivable and payable effects;
- settlement completion and clearing of the relevant settlement records.

The project will define event categories, required data, expected sequencing, acknowledgements, exceptions, and controls. It will not prescribe a universal accounting treatment, implement a general ledger, define a production chart of accounts, or claim compliance with a specific framework.

The detailed accounting policy remains subject to `OQ-005` and will be explicitly labeled as illustrative.

## 13. Reconciliation and Control Scope

The project includes the following control objectives:

- source-to-integration receipt completeness;
- integration-to-Fund-Accounting delivery and acknowledgement completeness;
- current trade-version alignment between source, integration, and accounting records;
- eligible-trade valuation completeness by valuation date;
- accepted valuation consistency between valuation and accounting records;
- maturity population completeness for the processing data;
- required accounting-event completeness following valuation and maturity;
- settlement instruction completeness for both currency legs;
- settlement amount, currency, due-date, and status consistency;
- explicit identification of unresolved breaks before NAV-readiness confirmation.

The SQL implementation will demonstrate selected data-quality and integration-reconciliation controls using synthetic data. It will not reproduce the generic reconciliation engine or exception-management user interface already covered by other portfolio projects.

## 14. Exception and Recovery Scope

The project will distinguish at least the following  exception classes:

| Exception class | Typical handling within scope |
|---|---|
| Request or schema error | Reject synchronously with a deterministic error response |
| Business-validation error | Reject or record a non-retryable business exception requiring corrected data |
| Duplicate request | Return the defined idempotent outcome without creating duplicate business effects |
| Version conflict | Reject the stale or invalid version and preserve the current accepted version |
| Retryable technical failure | Retry under a defined policy and retain each attempt in the audit trail |
| Downstream business rejection | Record the destination, rejection reason, ownership, and correction path |
| Missing scheduled data | Create a valuation, maturity, settlement, or control exception after the agreed cut-off |
| Reconciliation break | Record the failed control, compared values, owner, and required investigation path |

The case study will specify ownership, severity, status, retry eligibility, escalation, and audit requirements. It will not build a full case-management workflow, operational queue interface, or notification platform.

## 15. Reporting Scope

The project will define operational outputs for:

- lifecycle events received, accepted, rejected, distributed, and failed;
- processing status by trade, version, event, correlation identifier, and destination;
- unresolved and aged exceptions by category and owner;
- active trades missing an accepted valuation for the required date;
- trades approaching or reaching maturity;
- settlement status by trade and currency leg;
- outstanding reconciliation breaks;
- NAV-readiness status and blocking exceptions.

These outputs may be demonstrated through API responses, CSV examples, or SQL query results. A production reporting platform, financial-performance dashboard, and Streamlit user interface are out of scope.

## 16. Analysis Scope Versus Runnable Implementation

| Capability | Documentation and analysis | Runnable portfolio implementation |
|---|---|---|
| New trade | Full process, requirements, mapping, API, error, control, and UAT coverage | Implemented in FastAPI |
| Validation | Full validation catalogue and expected outcomes | Selected schema, reference, cross-field, date, and lifecycle rules implemented |
| Idempotency and versioning | Full behavioral specification | Implemented for supported lifecycle requests |
| Amendment | Full analysis including invalid-state handling | Implemented for eligible pre-maturity trades |
| Cancellation | Full analysis including stale-version and downstream-failure branches | Implemented for eglible pre-maturity trades |
| Daily valuation | Full population, ingestion, error, and control analysis | Bulk validation ingestion and selected completeness checks implemented |
| Maturity and accounting reclassification | Full target-state sequencing and control analysis | Simplified scheduled-processing simulation and accounting-event records |
| Settlement | Full two-leg status, exception, and reconciliation analysis | Synthetic instructions and status updates; no payment execution |
| Reconciliation | Full definition of selected integration controls | SQL checks and synthetic reconciliation results |
| Reporting | Data requirements and expected operational outputs | Selected API, CSV, or SQL outputs: no dashboard |
| External systems | Interface contracts and expected responses | Mocked acknowledgements and failure outcomes only |
| Audit and processing status | Full requirements and data definitions | Implemented for the selected flows |

The code exists to demonstrate that selected specifications are implementable and testable. It is not intended to reproduce the internal behaviour of commercial trade, accounting, settlement, reconciliation, or NAV platforms.

## 17. Scenario Coverage

| Scenario ID | Scenario | Analysis coverage | Planned executable evidence |
|---|---|---|---|
| `SCN-001` | New FX Forward successfully validated and distributed | End-to-end happy path | API test, processing records, mocked acknowledgements, and control result |
| `SCN-002` | New trade rejected due to business validation failure | Synchronous and business-error branches | API validation test and error response |
| `SCN-003` | Trade amendment including duplicate and stale-version handling | Version, idempotency, and downstream-status branches | Amendment API tests and immutable version records |
| `SCN-004` | Trade cancelled before maturity | Valid and invalid-state cancellation branches | Cancellation API tests and donwstream processing records |
| `SCN-005` | Daily valuation received with a missing-valuation exception branch | Valuation population, ingestion, cut-off, and control flow | Valuation API test and SQL completeness check |
| `SCN-006` | Trade matures, accounting is reclassified and one settlement leg fails | Maturity, accounting, two-leg settlement, error, and reconciliation branches | Simplified maturity run, settlement update, and SQL control result |

No scenario may be marked complete solely because a diagram or narrative exists. Completion requires the relevant requirement, data, errors, controls, and UAT coverage.

## 18. Explicit Out-of-Scope Items

| Exclusion ID | Out-of-scope item | Reason for exclusion |
|---|---|---|
| `OOS-001` | NDFs, FX swaps, options, spot trades, and other OTC products | Protects the one-product, deep-lifecycle approach |
| `OOS-002` | Investment decision, order management, execution, price negotiation, confirmation, and affirmation | These activities occur before the defined process boundary |
| `OOS-003` | Pricing engine, market-data sourcing, curves, and valuation-model implementation | Valuations are supplied by `SYS-VAL` |
| `OOS-004` | Collateral, margin, counterparty exposure, and credit-limit management | Separate business capabilities not required by the selected scenarios |
| `OOS-005` | Settlement netting, CLS, SWIFT generation, bank connectivity, and settlement-instruction maintenance | The project monitors settlement but does not execute payments |
| `OOS-006` | Counterparty onboarding, KYC, AML, sanctions screening, and legal-document management | Separate onboarding and compliance domain |
| `OOS-007` | Regulatory, tax, transaction, and investor reporting | Not required to demonstrate the selected integration capability |
| `OOS-008` | Full accounting engine, chart of accounts, NAV calculation, and financial-statement production | Only accounting-event integration is required |
| `OOS-009` | Generic reconciliation engine, exception dashboard, and full operational case management | Already represented elsewhere in the portfolio and would duplicate scope |
| `OOS-010` | Partial termination, novation, early settlement, post-maturity amendment, and complex backdated correction | High-complexity lifecycle variants deferred from the core case study |
| `OOS-011` | Production migration, cutover, parallel run, and historical-data conversion | Existing active trades may appear in synthetic data, but migration is not designed |
| `OOS-012` | Production identity management, network design, infrastructure, deployment, disaster recovery, and operational support implementation | Non-functional expectations may be documented, but no production environment exists |
| `OOS-013` | Real vendor integrations, employer data, proprietary schemas, and confidential procedures | Required to keep the case study generic and safe for public use |
| `OOS-014` | Production BI platform, financial-performance dashboard, or Streamlit application | Reporting is demonstrated through contracts and data outputs only |
| `OOS-015` | Fabricated stakeholder interviews, approvals, production results, or regulatory sign-off | The project must not misrepresent portfolio work as real delivery experience |

## 19. Delivery Artefacts in Scope

The project includes the following artefact groups:

| Artefact group | Included outputs |
|---|---|
| Business context | Business Case, Scope, Stakeholder Analysis, Assumptions/Constraints/Open Questions Register and Glossary |
| Process and architecture | Current-State Process, Target-State Process, BPMN, System Context, Data Flow, Lifecycle Model, Sequence Scenarios, and Architecture Decision Log |
| Requirements | Requirements Catalogue, User Stories, Acceptance Criteria, Business Rules, and Non-Functional Requirements | 
| Data and integration | Interface Inventory, Canonical Data Dictionary, Logical Data Model, Source-to-Target Mapping, API Functional Specification, OpenAPI, JSON examples, Error Catalogue, and Validation Matrix |
| Testing and controls | UAT Plan, UAT Test Cases, Traceability Matrix, Risk and COntrols Matrix, SQL controls, and synthetic sample data |
| Delivery evidence | Jira-style Product Backlog and BA-developer-QA collaboration model |
| Runnable evidence | Minimal FastAPI Integration Layer, automated tests, and Python consistency-validation scripts |
| Optional automation | GitHub Actions validation workflow after the local validation commands are stable |

Each artefact will be created and reviewed incrementally. Empty placeholder documents are not considered delivery progress.

## 20. Non-Functional Analysis Scope

The project will define requirements for:

- availability and processing cut-offs;
- expected volume, throughput, and response times;
- reliability, idempotency, retry, and recovery;
- authentication, authorisation, and secure transport;
- auditability; traceability, and data retention;
- monitoring, correlation, alerting, and support diagnostics;
- API and schema versioning;
- maintainability and separation of system responsibilities;
- business-date, time-zone, and currency-calendar handling;
- synthetic-data privacy and prevention of confidential-data exposure.

The mock implementation will demonstrate only selected behaviours, such as idempotency, deterministic validation, audit records, and processing status. Production load, penetration, failover, resilience, and disaster-recovery testing are out of scope.

## 21. Portfolio Completion Criteria

| Criterion ID | Completion criterion |
|---|---|
| `EXIT-001` | All six baseline scenarios are represented consistently across process, system, data, interface, error, control, and UAT artefacts. |
| `EXIT-002` | Every approved requirement has a valid identifier, source objective, status, priority, and traceability to relevant tests. |
| `EXIT-003` | The canonical data dictionary, source-to-target mappings, OpenAPI contract, and JSON examples use consistent field names, 
| `EXIT-004` | The OpeanAPI contract and example payloads pass the selected automated validatos. |
| `EXIT-005` | The mock API passes automated tests for supported happy paths, validation errors, duplicates, stale versions, and selected downstream failures. |
| `EXIT-006` | SQL controls produce the expected pass and break results for documented synthetic scenarios. |
| `EXIT-007` | Automated consistency checks detect duplicate identifiers, invalid references, missing mandatory fields, and orphaned traceability entries. |
| `EXIT-008` | The repository clearly identifies all fictional assumptions, implementation limitations, and out-of-scope production capabilities. |
| `EXIT-009` | No confidential, employer-specific, personal, or real transaction data is included. |
| `EXIT-010` | The README provides a concise walkthrough that allows a reviewer to follow at least one happy path and one failure path from business objective to executable evidence. |

## 22. Dependencies and Constraints Applicable to Scope

This scope inherits dependencies `DEP-001` to `DEP-007` and constraints `CON-001` to `CON-006` from `DOC-BC-001`.

The dependencies that mostly directly affect detailed design are:

- agreement on the canonical FX Forward definition;
- availability and ownership of reference data;
- downstream amendment and canellation behaviour;
- valuation-date and valuation-currency rules;
- authoritative settlement status;
- illustrative accounting policy;
- operational caneldars, cut-offs, and service levels.

An unresolved dependency may prevent a design component from moving beyond draft status, but it does not automatically expand the approved scope.

## 23. Facts, Assumptions, and Open Questions

### 23.1 Inherited Facts

The project facts `FACT-001` to `FACT-004` defined in `DOC-BC-001` remain unchanged. In particular, the project is a fictional portfolio case study and does not represent a completed or planned production implementation.

### 23.1 Inherited Assumptions

Assumptions `ASM-001` to `ASM-012` from `DOC-BC-001` remain part of the scope baseline. They establish the fictional setting, deliverable FX Forward product boundary, system ownership, immutable versions, REST and asynchronous processing semantics, external valuation, two-leg settlement, idempotency, illustrative accounting policy, and mock implementation boundary.

### 23.3 Additional Scope Assumptions

| Assumption ID | Assumption |
|---|---|
| `ASM-013` | Each in-scope trade belongs to exactly one fund and one investment account or portfolio and has exactly one counterparty. |
| `ASM-014` | Buy and sell directions are expressed from the fund's perspective. |
| `ASM-015` | An amendment and cancellation are supported only before the contractual value date. |
| `ASM-016` | Amendment and cancellation are supported only before the contractual value date. |
| `ASM-017` | Existing active trades may be included in synthetic sample data, but their migration into the target solution is not analysed. |
| `ASM-018` | The contractual value date is treated as the maturity and settlement date for the core case study. |
| `ASM-019` | Independent end-of-day source of control snapshots are available to the Reconciliation Platform. |

### 23.4 Open Questions

Open questions `OA-001` to `OQ-007` from `DOC-BC-001` remain open and retain their existing identifiers.

The following scope-specific questions are added:

| Open question ID | Open question | Required by |
|---|---|---|
| `OQ-008` | Which source fields may be amended without operational approval or a cancel-and-rebook instruction? | Business rules and target process |
| `OQ-009` | What cancellation cut-off and downstream statuses determine cancellation eligibility? | Lifecycle model and validation matrix |
| `OQ-010` | Is a final valuation required on the control value date before maturity reclassification? | Valuation and accounting requirements |
| `OQ-011` | Can a final settlement status be reversed or corrected, and how is a one-leg failure resolved? | Settlement sequence and error catalogue |
| `OQ-012` | Is reference data queried in real time, cached by the Integration Layer, or supplied through a scheduled feed? | Interface Inventory and non-functional requirements |

No open question may be silently resolved in code. The agreed answer must first be recorded in the relevant decision, requirement, rule, or interface artefact.

## 24. Scope Change Control

A proposed addition of a product, lifecycle event, system, interface, data domain, scenario, report, or runnable capability requires explicit scope assessment.

Each proposed scope change must:

1. receive a unique change-request identifier;
2. state the business objective and expected benefit;
3. identify affected processes, systems, interfaces, data mappings, requirements, controls, tests, and delivery artefacts;
4. assess whether the change duplicates another portfolio project;
5. identify new assumptions, dependencies, risks, and open questions;
6. update this document before implementation begins.

An idea recorded in the backlog is not automatically in scope.

## 25. Related and Planned Artefacts

| Document ID | Artefact | Relationship to scope |
|---|---|---|
| `DOC-BC-001` | Business Case | Defines the problem, objectives, benefits, and recommended option |
| `DOC-STK-001` | Stakeholder Analysis | Defines the roles participating in the in-scope process and decisions |
| `DOC-AOQ-001` | Assumptions, Constraints, and Open Questions Register | Becomes the authoritative register for unresolved scope inputs |
| `DOC-CSP-001` | Current-State Process | Documents the assumed process within the start and end boundaries |
| `DOC-TSP-001` | Target-State Process | Describes the target operating flow for the in-scope abilities |
| `DOC-SCTX-001` | System Context | Expands the logical system responsibilities defined in this document |
| `DOC-LCY-001` | FX Forward Lifecycle Model | Defines status dimensions and permitted transitions |
| `DOC-REQ-001` | Requirements Catalogue | Converts each approved capability into testable requirements |
| `DOC-IF-001` | Interface Inventory | Expands interfaces `IF-001` to `IF-016` |
| `DOC-TRC-001` | Traceability Matrix | Demonstrates coverage from objectives and scope through UAT evidence |
