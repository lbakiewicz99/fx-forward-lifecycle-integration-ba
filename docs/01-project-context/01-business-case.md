# FX Forward Trade Lifecycle Integration - Business Case

## Document COntrol

| Field          | Value                                                             |
|----------------|-------------------------------------------------------------------|
| Document ID    | `DOC-BC-001`                                                      |
| Project        | FX Forward Trade Lifecycle Integration BA Case Study              |
| Repository     | `fx-forward-lifecycle-integration-ba`                             |
| Version        | 0.1                                                               |
| Status         | Draft for review                                                  |
| Date           | 2026-07-17                                                        |
| Document Owner | Business Analysis                                                 |
| Classification | Public portfolio artefact - fictional scenario and synthetic data |

### Version History

| Version | Date | Change | Author |
|---|---|---|---|
| 0.1 | 2026-07-17 | Initial business case | Portfolio author |

## 1. Purpose

This document presents the business case for introducing an automated controlled integration capability for the FX Forward trade lifecycle within a fictional investment organisation.

The proposed capability connects the Trade Booking System, Integration Layer, Fund Accounting Platform, Valuation Service, Settlement/Cash Gateway, Rconciliation Platform, and NAV Reporting Layer. It is intended to reduce manual handling, improve lifecycle data consistency, provide end-to-end processing visibility, and support timely accounting and NAV-related activities.

This document establishes why the change is needed and what business outcomes it is expected to support. Detailed scope boundaries, requirements, interface specification, process models, and solution design will be maintained in separate project artefacts.

> **Case study notice:** This is a documentation-led portfolio case study. It does not describe a real organisation, production implementation, vendor configuration, or employer process. All examples, systems, data, controls, and operating assumptions are fictional or generic.

## 2. Executive Summary

The fictional organisation uses FX Forwards to manage investment and currency exposures across its funds. A single FX Forward may affect trade records, daily valuation, unrealised and realised profit and loss, balance sheet classification, maturity processing, settlement, reconciliation, and NAV reporting.

The working current-state assumption is that lifecycle information is exchanged through a combination of manual input, batch files, operational checks, and point-to-point communication. These hand-offs create a risk that new trades, amendments, cancellations, valuations, maturity events, or settlement statuses are processed late or differently across systems.

The recommended change is a canonical Integration Layer that receives supported FX Forward lifecycle events, validate them, mainatins an auditable processing record, and distributes consistent information to downstream systems. The Integration Layer will also support valuation ingestion, maturity processing, settlement-status monitoring, exception handling, and integration control reporting.

The recommended option is expected to provide the strongest balance of automation, control, traceability, and maintainability. It is preferred over retaining the assumed manual process or creating additional independent point-to-point interfaces.

## 3. Background and Business Context

FX Forwards are over-the-counter derivative contracts under which two parties agree to exchange specified currency amounts on a future date at an agreed rate. From an investment operations and fund accounting perspective, the trade must remain consistent across multiple lifecycle and accounting activities.

The relevant business activities include:

- recording agreed trade economics;
- validating the fund, counterparty, currencies, dates, amounts, and rate;
- distributing new trades and subsequent lifecycle changes;
- obtaining and recording daily valuations;
- monitoring trades appraoching maturity;
- revognising the accounting effect of valuation and maturity events;
- initiating and monitoring the settlement of both currency legs;
- reconciling source, accounting, valuation, and settlement records;
- identifying and resolving processing exceptions;
- confirming that required data and controls are complete before NAV reporting.

When these activities rely on separate manual or batch processes, the organisation may not have a single, timely view of whether a trade has been accepted and processed consistently by every required system.

## 4. Problem Statement

The fictional organisation does not have a common, automated, and traceable mechanism for processing FX Forward lifecycle information across its trade, valuation, accounting, settlement, reconciliation, and reporting systems.

The assumed current-state symptoms are:

| Current-state symptom | Potential business consequence |
|---|---|
| Trade details are manually re-entered or transformed between systems | Incorrect amounts, currencies, dates, rates, or identifiers |
| Validation rules differ between teams or systems | A trade may be accepted by one system and rejected by another |
| Amendments and cancellations are communicated through separate hand-offs | Downstream systems may retain outdated trade economics or status |
| Processing acknowledgements are not linked through a common correlation identifier | Operations teams cannot quickly determine where processing failed |
| Daily valuation completeness is checked separately from lifecycle processing | Missing valuations may be detected late in the NAV process |
| Maturity and settlement are monitored through manual schedules | A due trade or failed settlement leg may not be escalated promptly |
| Reconciliation occurs after downstream processing | Breaks are identified after they have already affected operational deadlines |
| Exception ownership and retry behaviour are inconsistent | Duplicate processing, unresolved breaks, or delayed corrections may occur |

There symptoms are hypotheses for the fictional case study and will be represented explicitly as assumptions until addressed by the current-state process analysis.

## 5. Business Need

The organisation requires a controlled integration capability that can:

1. receive and validate supported FX Forward lifecycle instructions;
2. preserve the identity and version of each trade throughout its lifecycle;
3. distribute consistent trade and valuation data to the required downstream systems;
4. provide an auditable status for every accepted instruction;
5. prevent duplicate or stale events from creating inconsistent downstream effects;
6. identify missing, rejected, delayed, or inconsistent processing outcomes;
7. support maturity, accounting reclassification, and two-leg settlement monitoring;
8. provide evidence that required lifecycle, valuation, accounting, settlement, and reconciliation activities are complete before reporting deadlines.

## 6. Business Objectives

| Objective ID | Objective | Intended outcome |
|---|---|---|
| `OBJ-001` | Automate the supported FX Forward lifecycle hand-offs | Reduce manual re-entry and operational dependency on email, spreadsheets, and disconnected batch checks |
| `OBJ-002` | Improve lifecycle data integrity across systems | Ensure that downstream systems receive a consistent, version-controlled representation of each trade |
| `OBJ-003` | Detect and classify processing exceptions earlier | Allow operations and support teams to identify the failed stage, cause, owner, and permitted recovery action |
| `OBJ-004` | Support timely valuation, accounting, settlement, and NAV activities | Identify incomplete or inconsistent processing before relevant operational cut-offs |
| `OBJ-005` | Strengthen auditability and operational control | Maintain traceable evidence of receipt, validation, distribution, acknowledgement, retry, and final processing outcome |
| `OBJ-006` | Establish maintainable integration contracts | Provide documented interfaces, cannonical data definitions, mappings, validation rules, and testable acceptance criteria |

## 7. Business Drivers

| Driver | Description |
|---|---|
| Operational efficiency | Reduce repetitive trade re-keying, manual status chasing, and cross-system investigation |
| Data quality | Apply consistent validation and mapping rules before data reaches accounting and reporting processes |
| NAV timeliness | Surface missing valuations, maturity-processing gaps, and settlement issues before NAV-related deadlines |
| Operational risk | Reduce the likelihood of outdated trade versions, duplicate processing, missed lifecycle events, and unowned exceptions |
| Audit and control | Create an end-to-end processing record linked by stable trade, event, and correlation identifiers |
| Change capability | Replace fragmented interface logic with reusable canonical contracts and explicit system responsibilites |
| Scalability | Allow increased trade volume to be handled without a proportional increase in manual checks |

## 8. Options Considered

### 8.1 Option Assesment

| Criterion | Option 0: Retain assumed current state | Option 1: Add point-to-point interfaces | Option 2: Introduce a canonical Integration Layer |
|---|---|---|---|
| Initial delivery effort | Low | Medium | Medium to High |
| Reduction in manual handling | Low | Medium | High |
| Cross-system data consistency | Low | Medium | High |
| End-to-end traceability | Low | Low to Medium | High |
| Support for common validation | Low | Medium | High |
| Maintainability as interfaces increase | Low | Low | High |
| Central retry and exception handling | Low | Low to Medium | High |
| Risk of duplicated transformation logic | High | High | Low |
| Strategic fit | Low | Medium | High |

### 8.2 Option 0 - Retain the Assumed Current State

This option would retain manual and batch-based hand-offs with local operational controls.

It requires the least immediate change but does not address fragmented validation, delayed exception detection, limited traceability, or dependency on manual investigation. It is not recommended.

### 8.3 Option 1 - Add Point-to-Point Interfaces

This option would create separate interfaces between the Trade Booking System and each downstream platform.

It may automate selected hand-offs more quickly, but each interface could implement its own data transformation, validation, retry, and error semantics. The resulting design would be harder to control and maintain as lifecycle events and consuming systems increase. It is not recommended as the target-state approach.

### 8.4 Option 2 - Introduce a Canonical Integration Layer

This option introduces a controlled integration capability with a canonical FX Forward representation, common validation rules, event versioning, idempotent processing, auditable distribution, and standard error handling.

It requires greater upfront analysis and contract definition but provides the strongest long-term support for consistency, traceability, control, and change. It is recommended option for this case study.

## 9. Recommended Business Capability

The proposed Integration Layer will provide the following high-level capabilities:

- receive new trade, amendment, and cancellation instructions through documented interfaces;
- validate message structure and defined business rules;
- identify duplicate requests and reject stale trade versions;
- map source data to a canonicla FX Forward mode;
- maintain event, trade-version, and correlation identifiers;
- distribute validated lifecycle information to downstream systems;
- provide processing status without treating initial acceptance as confirmation of downstream completion;
- send active trade data to the Valuation Service and receive daily valuation results;
- distribute accepted valuations to the Fund Accounting Platform;
- identify missing or rejected valuations for a required valuation date;
- detect maturity using an agreed business date and calendar;
- create the required maturity and accounting reclassification instruction;
- create and monitor separate buy-leg and sell-leg settlement records;
- receive settlement status updates from the Seeltment/Cash Gateway;
- supply processing and control data to the Reconciliation Platform;
- classify technical, business, data-quality, and control exceptions;
- support controlled retry or corrected resubmission where permitted;
- provide operational reports for processing completeness, upcoming maturities, unresolved exceptions, settlement status, and NAV readiness.

The Integration Layer will maintain an authoritative audit trail of integration processing. It will not replace the Trade Booking System as the source of truth for trade economics or the Fund Accounting Platform as the accounting book of record.

## 10. High-Level Scope Summary

### 10.1 In Scope

- deliverable FX Forwards with two currency legs;
- one fund, one investment account or portfolio, and one couterparty per trade;
- new trade, amendment, and pre-maturity cancellation;
- trade and event version control;
- reference-data and business-rule validation;
- canonical data mapping;
- daily valuation ingestion and completeness monitoring;
- maturity detection;
- illustrative accounting reclasiffication instructions;
- settlement instruction and leg-level status monitoring;
- integration completeness and consistency controls;
- exception classification, ownership, retry, and audit requirements;
- operational and NAV-readiness reporting;
- REST API contract, JSON examples, a minimal FastAPI mock, SQL controls, and synthetic sample data.

### 10.2 Out of Scope

- non-deliverable forwards, FX swaps, options, and spot trades;
- pricing-model implementation and market-data sourcing;
- trade confirmation or affirmation;
- collateral and margin management;
- settlement netting, CLS processing, and SWIFT message generation;
- counterparty onboarding and settlement-instruction maintenance;
- regulatory reporting;
- full general-ledger, fund-accounting, reconciliation, or NAV-engine implementation;
- post-maturity restructuring, novation, and complex backdated correction workflows;
- production authentication, infrastructure, deployment, disaster recovery, and vendor-specific configuration.

The detailed and authoritative scope will be maintained in `DOC-SCP-001`.

## 11. Stakeholder value

| Stakeholder group | Expected value |
|---|---|
| Trade Operations / Middle Office | Reduced manual hand-offs and clearer processing status for new, amended, and cancelled trades |
| Fund Accounting and NAV Operations | More timely and consistent trade, valuation, maturity, and settlement information |
| Valuation Operations / Oversight | Defend eligible-trade population and explicit missing or rejected valuation controls |
| Reconciliation and Operational Controls | Independent data feeds, defined control points, and standard break classification |
| Technology Support | Correlation identifiers, error categories, retry rules, and auditable processing trail |
| Integration Developers | Explicit source-to-target mappigns, interface results, and reusable test data |
| Quality Assurance | Traceable requirements, deterministic expected results, and reusable test data |
| Operations Management / Risk | Improved visibility of unresolved exceptions, control status, and operational readiness|

A detailed stakeholder analysis, including incluence, interest, responsibilites, and engagement approach, will be maintained in `DOC-STK-001`.

## 12. Expected Benefits

| Benefit ID | Expected benefit | Benefit type | Related objectives |
|---|---|---|---|
| `BEN-001` | Reduced manual entry and lifecycle-status chasing for supported events | Efficiency | `OBJ-001`, `OBJ-004` |
| `BEN-002` | More consistent trade economics and lifecycle status across systems | Data quality | `OBJ-002`, `OBJ-006` |
| `BEN-003` | Earlier identification of rejected, missing, delayed, or inconsistent processing | Risk reduction | `OBJ-003` , `OBJ-004` |
| `BEN-004` | Improved evidence that valuation, maturity, accounting, settlement, and control activities are complete | Control | `OBJ-004`, `OBJ-005` |
| `BEN-005` | Faster root-cause analysis using linked trade, event, version, and correlation identifiers | Operational support | `OBJ-003`, `OBJ-005` |
| `BEN-006` | Lower future change effort through reusable contracts and canonical definitions | Change capability | `OBJ-002`, `OBJ-006` |

Because this is a fictional case study without observed production volumes, staffing costs, incident history, or measured processing times, these benefits will not be assigned invented financial values. Quantitative benefit estimates would require a validated baseline and agreed measurement period.

## 13. Proposed Success Measures

The following are design targets for the fictional solution, not claims about measured production performance.

| Measure ID | Proposed measure | Target for the case study | Related objectives |
|---|---|---|---|
| `KPI-001` | Supported inbound lifecycle requests with an event ID, trade ID, version, correlation ID, and recorded receipt time | 100% | `OBJ-002`, `OBJ-005` |
| `KPI-002` | Accepted lifecycle events ending in either a defined downstream completion status or an explicit exception | 100% | `OBJ-003`, `OBJ-005` |
| `KPI-003` | Duplicate requests with the same idempotency identifier creating duplicate downstream business effects | 0 | `OBJ-002`, `OBJ-005` |
| `KPI-004` | Stale amendments overwriting a later accepted trade version | 0 | `OBJ-002`, `OBJ-005` |
| `KPI-005` | In-scope matured trades with either ac accepted valuation for the required date or a named valuation exception before the agreed cut-off | 100% | `OBJ-003`, `OBJ-004` |
| `KPI-006` | In-scope matured trades with either the required maturity/reclassification processing and two settlement-leg records or an explicit exception | 100% | `OBJ-003`, `OBJ-004` |
| `KPI-007` | Open exceptions with a category, owner, status, detection timestamp, and permitted recovery action | 100% | `OBJ-003`, `OBJ-005` |
| `KPI-008` | Approved requirements linked to relevant design components and UAT coverage | 100% | `OBJ-005`, `OBJ-006` |

Operational cut-off times, service levels, processing volumes, and ageing thresholds remain subject to discovery and will be documented as non-functional requirements or business rules where appropriate.

## 14. Business and Operating Impact

| Impact area | Expected change |
|---|---|
| Process | LIfecycle events move from disconnected hand-offs to a controlled end-to-end flow |
| Roles | Operations teams focus on exceptions and control evidence rather than routine re-keying and status chasing |
| Data | A canonical FX Forward model and stable identifiers are used across integration contracts |
| Technology | Downstream communication is managed through defined interfaces and processing states |
| Controls | Completeness, version, valuation, maturity, and settlement controls are designed into the flow |
| Support | Technical retries are separated from business corrections and manual investigation |
| Testing | Requirements, mappings, API operations, errors, and UAT results are connected through traceability |

The project does not assume that automation removes business ownership. Business and operations teams remain accountable for defining rules, investigating non-technical exceptions, approving corrections, and confirming operational readiness.

## 15. Cost and Effort Considerations

A credible financial appraisal cannot be produced without real volumes, current processing effort, incident costs, platform constraints, vendor charges, or implementation estimates. The case study will therefore use relative complexity rather than fictional monetary values.

The main delivery-effort drivers are expected to be:

- business and accounting policy definition;
- current-state discovery and target-state agreement;
- canonical model and mapping design;
- interface development and downstream adapter changes;
- reference-data availability and ownership;
- historical or in-flight trade migration decisions;
- integration, regression, and user-acceptance testing;
- operational control and support procedures;
- monitoring, alerting, and production-readiness activities.

The runnable portfolio implementation will indentionally cover only a selected vertical slice and will not represent the effort required for a production implementation.

## 16. Dependencies

| Dependency ID | Dependency | Why it matters |
|---|---|---|
| `DEP-001` | Agreed canonical FX Forward definition | Required for consistent validation, mapping, API contracts, and reconciliation |
| `DEP-002` | Reliable fund, currency, counterparty, and calendar reference data | Required to validate trade eglibility and determine lifecycle dates |
| `DEP-003` | Defined donwstream amendment and cancellation behaviour | Determines whether lifecycle changes can be applied directly or require compensating events |
| `Dep-004` | Agreed valuation interface and valuation-date rules | Required for daily valuation ingestion and completeness controls |
| `DEP-005` | Authoritative settlement-status source | Required to distinguis instructed, pending, failed, and completed settlement legs |
| `DEP-006` | Agreed fictional accounting policy | Required to specify valuation, maturity, reclassification, and settlement accounting events |
| `DEP-007` | Agreed operational calanedar and cut-offs | Required for maturity triggers, service levels, escalation, and NAV-readiness controls |

## 17. Constraints

| Constraint ID | Constraint |
|---|---|
| `CON-001` | The project must use only fictional organisations, generic system names, and synthetica data. |
| `CON-002` | The project must not reproduce confidential employer processes, vendor schemas, or proprietary configuration. |
| `CON-003` | The functional scope is limited to deliverable FX Forwards. |
| `CON-004` | Only the Integration Layer will receive a minimal runnable implementation. Other systems will be represented through contracts, sample data, and mocked outcomes. |
| `CON-005` | The case study will not claim production deployment, regulatory approval, or universal accounting correctness. |
| `CON-006` | Financial benefits and implementation costs will remain unquantified unless a credible fictional baseline is explicitly defined. |

## 18. Initial Risks

| Risk ID | Risk | Potential impact | Initial resposne |
|---|---|---|---|
| `RISK-001` | Scope expand into additional OTC products or full platform implementation | The case study becomes too broad to complete or review | Enforce the deliverable FX Forward boundary and selected vertical slice |
| `RIKS-002` | Accounting treatment is left ambiguous or presented as universally applicable | Requirements and tests become inconsistent or misleading | Document an explicit fictional accounting policy and its limitations |
| `RISK-003` | Trade, event, and version identifiers are used inconsistently | Duplicate, stale, or mismatched processing cannot be controlled | Define identifier ownership and version rules before interface design |
| `RISK-004` | Initial API acceptance is interpreted as downstream completion | Users may believe a trade is fully processed when a downstream system has failed | Define separate acknowledgement and processing-status semantics |
| `RISK-005` | Reference-data or business-calendar dependencies are understated | Validation and maturity processing produce unreliable results | Define authoritative sources, fallback behaviour, and exception handling |
| `RISK-006` | Reconciliation relies only on data already transformed by the Integration Layer | Missing source events may not be detected independently | Use independent source snapshots or control totals where appropriate |
| `RISK-007` | Requirements, mappings, OpeanAI, examples, code, and tests drift apart | The repository loses credibility as an analysis artefact | Introduce automated cross-artefact validation and scheduled consistency reviews |
| `RISK-008` | The mock API is mistaken for a production-ready service | The project overstates its implementation maturity | Label the implementation and documnentation as a controlled portfolio mock |

Risks will be expanded with likelihood, impact, ownership, controls, and residual assesment in the Risk anc Contols Matrix.

## 19. Facts, Assumptions, and Open Questions

### 19.1 Project Facts

| Fact ID | Fact |
|---|---|
| `FACT-001` | The project is intended to demonstrate Business Analyst and Business System Analyst Capabilities in financial services. |
| `FACT-002` | The defined system landscape includes trade booking, integration, valuation, fund accounting, reconciliation, settlement, and NAV / reporting capabilities. |
| `FACT-003` | The planned artefacts include process, system, data, API, requirements, testing, control, and traceability deliverables. |
| `FACT-004` | The project does not represent a completed or planned production implementation. |

### 19.2 Working Assumptions

| Assumption ID | Assumption |
|---|---|
| `ASM-001` | The case study is fully fictional and uses only synthetic data. |
| `ASM-002` | Only deliverable FX Forwards are supported. |
| `ASM-003` | The Trade Booking System is the source of truth for trade economics and lifecycle instructions. |
| `ASM-004` | Amendments create immutable trade versions and previous accepted versions remain auditable. |
| `ASM-005` | Lifecycle requests use REST interfaces while downstream distribution follows asynchronous processing semantics. |
| `ASM-006` | A successful API acknowledgement confirms acceptance for processing, not successful completion in all downstream systems. |
| `ASM-007` | Valuations are calculated externally and delivered through the Integration Layer. |
| `ASM-008` | Settlement consists of two currency legs that may have different processing outcomes. |
| `ASM-009` | Delivery follows at-least-once semantics and uses idempotency controls to prevent duplicate business effects. |
| `ASM-010` | Accounting examples follow an agreed fictional policy and are not presented as universal treatment. |
| `ASM-011` | The FastAPI application mocks only the Integration Layer and selectged downstream responses. |
| `ASM-012` | SQLite may be used for the lightweight mock while the logical design remains technology-neutral. |

### 19.3 Open Questions

| Open question ID | Open question | Required by |
|---|---|---|
| `OQ-001` | What are expected event volumes, peak loads, operational cut-offs, and service levels? | Non-functional requirements |
| `OQ-002` | Does the Fund Accounting Platform support a ntive amendment, or must selected changes be translated into cancle-and-rebook processing? | Target process and interface design |
| `QO-003` | Which valuation currencies and valuation metadate are required by the Fund Accounting Platform and NAV process? | Canonical model and valuation interface |
| `QO-004` | Which system provides the authoritative settlement confirmation for each currency leg? | Settlement and reconciliation design |
| `QO-005` | Which accounting events and posting categories are required at valuation, maturity, reclassification, and settlement? | Accounting requirements and test cases |
| `QO-006` | Which validations must complete before API acknowledgement and which may complete asynchronously? | API and error-handling design |
| `QO-007` | How are business dates, currency holidays, and time zones applied to maturity and cut-off processing? | Business rules and non-functional requirements | 

The dedicated assumptions and open-questions register will become the authoritative source when `DOC-AOQ-001` is created. Until then, this section is the baseline register.

## 20. Recommendation

Processed with the canonical Integration Layer option and continue to detailed scope definition.

The next analysis activities are to:

1. confirm the supportred product and lifecycle boundaries;
2. define the system and organisational boundaries;
3. identify impacted stakeholders and decision owners;
4. document the assumed current state and its control gaps;
5. resolve or formally defer the open questions that affect target-state design.

This recommendation authorities continuation of the fictional case-study analysis only. It is not an investment approval, production-design approval, or accounting-policy approval.

## 21. Related and Planned Artefacts

| Document ID | Artefact | Relationship |
|---|---|---|
| `DOC-SCP-001` | Scope | Defines authoritative in-scope and out-of-scope boundaries |
| `DOC-STK-001` | Stakeholders Analysis | Defines stakeholder interests, responsibilities, and engagement approach |
| `DOC-AOQ-001` | Assumptions, Constraints, and Open Question Register | Maintains the controlled decision baselina |
| `DOC-CSP-001` | Current-State Process | Tests the current-state hypotheses stated in this business case |
| `DOC-TSP-001` | Target-State Process | Describe how the reommended capability changes the operating process |
| `DOC-REQ-001` | Requirements Catalogue | Translates the business objectives into testable requirements |
| `DOC-TRC-001` | Traceability Matrix | Links objectives, requirements, rules, interfaces, controls, and tests |