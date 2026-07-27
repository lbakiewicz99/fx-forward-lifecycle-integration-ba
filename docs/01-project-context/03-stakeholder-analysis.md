# FX Forward Trade Lifecycle Integration BA Case Study

## Stakeholder Analysis

| Field | Value |
|---|---|
| Document ID | DOC-STK-001 |
| Version | 0.1 |
| Status | Draft - portfolio case study |
| Document owner | Portfolio Author |
| Related documents | DOC-BC-001 - Business Case; DOC-SCP-001 - Scope |
| Last updated | 2026-07-22 |

## 1. Purpose

This document identifies the role-based stakeholder groups affected by, or capable of influencing, the proposed FX Forward lifecycle integration.

It defines each group's desired outcome, specialist input, decision rights, concerns, influence, interest, and expected involvement in analysis, design, validation, testing, and acceptance.

The analysis support:

- targeted elicitation and review activities;
- clear business, data, system, control, and test ownership;
- early identification of conflicting expectations;
- appropriate escalation and decision-making;
- selection of business representatives for UAT;
- traceability between stakeholder needs and later project artefacts.

Detailed system behaviour is documented separately in process models, requirements, business rules, data mappings, interface specifications, and acceptrance criteria.

## 2. Case-study status and boundaries

This is a fictional portfolio case study. It demonstrates an analysis approach and does not represent a production implementation.

No real organisation, client, employer, system configuration, stakeholder interview, approval, or production data is represented. Stakeholder names are role-based and the decision rights below describe an assumed governance model.

The analysis covers stakeholders relevant to the integration of:

- `SYS-TBS` - Trade Booking System;
- `SYS-INT` - Integration Layer;
- `SYS-RDS` - Reference Data Service;
- `SYS-VAL` - Valuation Service;
- `SYS-FAP` - Fund Accounting Platform;
- `SYS-SET` - Settlement/Cash Gateway;
- `SYS-REC` - Reconciliation Platform;
- `SYS-NAV` - NAV and Reporting Layer.

Systems are not stakeholders. Their users, business owners, data owners, engineering teams, system owners, control functions, and support teams are the stakeholders represented in this document.

## 3. Analysis approach

In a real delivery, the stakeholder register would be established and validated using:

- the organisation structure and existing responsibility matrices;
- process, system, data-domain, and control ownership records;
- interviews with the Sponsor, process owners, product owners, and SMEs;
- cross-functional process and interface workshops;
- review of incident, exception, and change-management records;
- confirmation of approval, escalation, and UAT responsibilities.

Each stakeholder group is assessed from two perspectives:

1. **Business perspective** - operational outcome, process impact, control ownership, business acceptance, and risk.
2. **System perspective** - system boundaries, data semantics, interfaces, technical constraints, testing, security, and supportability.

## 4. Established case-study facts

- The case study concerns deliverable FX Forwards with two currency legs.
- The lifecycle includes new trade, amendment, cancellation, daily valuation, maturity, settlement, accounting reclassification, reconciliation, exception handling, and reporting.
- `SYS-INT` is the only component intended to have a minimal runnable implementation. Other systems are represented through contracts, mocks, or sample data.
- The target process must include both successful processing and error or exception scenarios.
- The project must demonstrate collaboration between business stakeholders, developers, and QA.

## 5. Assumptions

| Assumption ID | Assumption |
|---|---|
| `STK-ASM-001` | Stakeholder groups represent functional responsibilities rather than fixed organisational teams. One individual or team may perform multiple stakeholder roles. |
| `STK-ASM-002` | The Business Owner and Project Sponsor are represented as one stakeholder group for this case study |
| `STK-ASM-003` | Front Office representation and TBS product ownership are represented as one stakeholder group. This allocation would require validation in a real organisation. |
| `STK-ASM-004` | A dedicated QA/Test Analyst role is represented even though some business validation and UAT activities may be performed by operational teams. |
| `STK-ASM-005` | Each participating system has an accountable owner who can confirm constraints and authorise changes to that system. |
| `STK-ASM-006` | Decision rights describe the proposed governance model and do not evidence an actual approval or production sign-off. |
| `STK-ASM-007` | Information Security / Technology Risk is consulted at specification and control-review level. The mock API is not presented as production-grade security implementation. |
| `STK-ASM-008` | `SYS-TBS` is the system of record for trade economics and booking lifecycle instructions, but not for valuation, accounting, reconciliation, or settlement status. |

## 6. Stakeholder register

Influence and interest ratings are indicative case-study assesments would require validation in a real organisation.

| Stakeholder ID | Stakeholder group | Representative roles | Influence | Interest | Engagement strategy |
|---|---|---|---|---|---|
| `STK-001` | Business Owner/Project Sponsor | Sponsor, Business Owner, escalation authority | High | High | Manage closely |
| `STK-002` | Front Office/Trade Booking System Product Owner | Source-system Business Owner, TBS Product Owner, source-data SME | High | High | Manage closely |
| `STK-003` | Trade Operations/Middle Office | Process Owner, operational SME, UAT participant | High | High | Manage closely |
| `STK-004` | Settlement Operations | Settlement Processor Owner, settlement SME, UAT participant | Medium | High | Involve regularly |
| `STK-005` | Valuation Operations/Valuation Oversight | Valuation SME, control owner, UAT participant | Medium | High | Involve regularly |
| `STK-006` | Fund Accounting and NAV Operations | Accounting SME, NAV Process Owner, UAT participant | High | High | Manage closely |
| `STK-007` | Reconciliation and Operational Controls | Reconciliation SME, Control Owner, UAT participant | Medium | High | Involve regularly |
| `STK-008` | Operational Risk/Internal Controls | Operational Risk Reviewer, control-governance representative | High | Medium | Keep satisfied and consult at control gates |
| `STK-009` | Reference Data Owner | Data Owner, data steward, mapping SME | Medium | Medium | Consult on data decisions |
| `STK-010` | Integrate Engineering | Integration Lead, API Developer, technical designer | High | High | Collaborate continously |
| `STK-011` | Participating System Owners | Application Owner, Product Owner, release authority | High | Medium | Keep satisfied and involve at interface or release gates |
| `STK-012` | Application Support | Support Lead, incident coordinator, monitoring SME | Medium | Medium | Consult on supportability and readiness |
| `STK-013` | QA/Test Analyst | Test Lead, Integration tester, defect coordinator | Medium | High | Involve from requirements through test completion |
| `STK-014` | Information Security/Technology Risk | Security Reviewer, technology-control SME | High | Medium | Consult at security design and exception gates |

## 7. Detailed stakeholder analysis

| ID | Desired outcome | Knowledge and input | Decision rights | Main concern |
|---|---|---|---|---|
| `STK-001` | Deliver a controlled integration capability that reduces manual processing and operational risk while supporting timely and accurate FX Forward lifecycle processing. | Strategic business priorities, high-level business needs, applicable SLAs, delivery constraints, expected benefits, and organisational risk tolerance. | Approves project scope and release priorities, resolves cross-functional conflicts, makes the final go/no-go decision, and accepts or escalates residual business risk, within the assumed governance model. | A rushed or poorly prioritised release may fail to deliver expected benefits, introduce new operational risks, or leave amendment and cancellation events dependent on inadequate manual controls. |
| `STK-002` | Ensure that FX Forward lifecycle events originating from `SYS-TBS` are complete, unambiguous, correctly versioned, and interpreted consistently by downstream systems. | Trade-booking workflows, source-field definitions, lifecycle-event and status semantics, trade identification and versioning rules, amendment and cancellation behaviour, resend behaviour, and source-system limitations. | Approves the business meaning of source fields, lifecycle-event and status definitions, source-interface changes, TBS change priorities, and TBS-related acceptance criteria. | Duplicate or out-of-sequence messages may be interpreted as new trades, while amendments or cancellations may be applied to the wrong trade version, causing downstream records to diverge from `SYS-TBS` as the system of record. |
| `STK-003` | Reduce manual handling and ensure timely and consistent processing of new trades, amendments, and cancellations. | Current-state lifecycle workflow, source-data variations, operational cut-offs, recurring processing issues, manual correction procedures, and escalation paths. | Approves the target operational workflow, lifecycle-exception ownership, manual-intervention rules, and Trade Operations UAT acceptance. | Incomplete or inconsistent source data and limited processing visibility may delay correction of failed lifecycle events.|
| `STK-004` | Ensure that both currency legs are instructed accurately, monitored independently, and resolved before relevant operational cut-offs. | Required settlement data, cut-off times, leg-level statuses, failure scenarios, correction procedures, and authoritative confirmation sources. | Approves the settlement workflow, required settlement data, status and escalation rules, and Settlement Operations UAT acceptance. | Incorrect amounts, currencies, or value dates, and a failure of only one settlement leg, may cause cash, accounting, reconciliation, and NAV breaks. |
| `STK-005` | Ensure that complete, timely, and valid daily FX Forward valuations are available for downstream accounting and NAV processing. | Valuation eligibility rules, valuation dates and currencies, required valuation atrtibutes, operational cut-offs, quality checks, exception procedures, and SLA requirements. | Approves valuation data requirements, valuation-control rules, exception-escalation criteria, and Valuation UAT acceptance. | Missing, stale, duplicate, rejected, or incorrectly denominated valuations may delay NAV processing or produce incorrect accounting results. |
| `STK-006` | Ensure accurate and timely processing of trade, valuation, maturity, settlement, and accounting events required for NAV production. | FX Forward accounting treatment, lifecycle-event timing, valuation requirements, posting categories, NAV cut-offs, accounting controls, and expected exception impacts. | Approves accounting business rules, expected accounting outputs, NAV-blocking criteria, exception treatment, and Fund Accounting UAT acceptance. | Incorrect mapping of currencies, amounts, signs, valuation versions, or lifecycle efents may produce incorrect asset or liability balances, realised or unrealsied P&L, and material NAV or financial-reporting impacts. |
| `STK-007` | Reduce manual investigation by consistently identifying lifecycle, valuation, accounting, and settlement breaks. | Reconciliation populations, matching keys, tolerances, cut-offs, recurring timing differences, break classifications, investigation procedures, and ownership rules. | Approves reconciliation rules, tolerance, break classifications, exception ownership, control evidence, and UAT acceptance. | Expected timing differences may be classified as genuine breaks, while genuine breaks may be incorrectly suppressed as expected differences. |
| `STK-008` |  Ensure that the target process reduces identified operational risks and produces transparent, testable control evidence. | Recurring operational incidents, existing manual controls and workarounds, risk classifications, control requirements, escalation procedures, and residual-risk criteria. | Reviews and endorses control design, evidence requirements, exception-escalation rules, and residual-risk treatment. | Automation may conceal reccuring failures or create false assurance if controls and exceptions are noit independently observable. |
| `STK-009` | Ensure that authoritative, current, and consistently mapped reference data is available for validating FX Forward events across participating systems. | Reference-data domains and ownership, fund and account structures, counterparty identifiers, permitted currencies and currency pairs, business calendars, cross-system mappings, effective-dating rules, and data-maintenance procedures. | Approved reference-data definitions, permitted values, mapping rules, ownership rules, effective-dating requirements, and reference-data change controls. | Missing, stale, or inconsistent reference data may cause valid trades to be rejected, invalid trades to be accepted, or the same trade to be represented differently across systems. |
| `STK-010` | Establish stable, versioned, and unambiguous interfaces that allow lifecycle events to move consistently between participating systems. | Integration feasibility, interface protocols, payload constraints, source-system behaviour, dependency failures, schema versioning, retry mechanisms, idempotency, security, and observability requirements. | Approves technical interface design, integration patters, schema conventions, implementation feasibility, and technical non-functional requirements. | Amiguous data ownership, inconsistent definitions, breaking schema changes, or non-idempotent retries may create duplicate or incorrectly processed lifecycle events. |
| `STK-011` | Ensure that integration changes remain compatible with each participating system and can be released without compromising security, availability, performance, or existing consumers. | System capabilities and constraints, interface contracts, supported identifier formats, authentication requirements, rate limits, processing window, capacity limits, dependencies, release calendars, monitoring capabilities, and recovery procedures. | Approve changes to the owned system and its interfaces, confirms system-specific technical constraints, authorises deployment windows, and confirms release readiness for the owned component. | Excessive request volumes, incompatible contract changes, or uncoordinated releases may cause service degradation, failed processing, or disruption to existing consumers. |
| `STK-012` | Ensure that integration failures can be detected, diagnosed, assigned, and recovered without relying on undocumented knowledge. | Historical incidents, common system failure modes, monitoring capabilities, existing support procedures, recovery constraints, and escalation paths. | Approves the operational support model, monitoring and alerting requirements, support runbooks, recovery procedures, and production-readiness criteria. | Insufficient observability or unclear ownership may increase resolution time, while automated retries may conceal recurring failures. |
| `STK-013` | Ensure that the integrated lifecycle solution is testable and that functional, integration, error-handling, and control requirements are verified with traceable evidence before business acceptance. | Test-design techniques, positive and negative scenarios, boundary testing, API and integration testing, test-data requirements, environment constraints, regression testing, defect classification, and requirements traceability. | Approves the test appraoch and QA coverage, confirm whether QA exit criteria have been met, and provides a release-readiness recommendation based on test results and unresolved defects. | Ambiguous acceptance criteria, incomplete scenario coverage, inadequate test data, or unresolved high-severity defects may create false confidence and allow critical failures to reach production. |
| `STK-014` | Ensure that service-to-service integrations protect trade data and follow approved authentication, authorisation, secrets-management, logging, and access-control-standards. | Security policies, approved authentication and authorisation patterns, secrets management, network controls, audit-logging requirements, vulnerability management, and security-incydent requirements. | Reviews and approves security requirements, security exceptions, and relevant security-control evidence within the assumed governance model. | Weak access controls, exposed credentials, excessive logging, or unauthorised API use may compromise confidentiality, integrity, or availability. |

## 8. Decision-right boundaries

Domain approval does not equal overall release approval. The proposed decision sequence is:

| Decision area | Accountable or confirming stakeholder | Boundary |
|---|---|---|
| Business scope, priorities, unresolved cross-functional conflicts, and final go/no-go | `STK-001` | Final business decision after receiving domain, QA, support, system-owner, and risk input. |
| TBS field meaning, lifecycle semantics, source versioning, and TBS change priority | `STK-002` | Limited to source-system business semantics and TBH changes. |
| Trade Operations workflow and business acceptance | `STK-003` | Accepts Trade Operations outcomes; does not approve technical architecture. |
| Settlement workflow and business acceptance | `STK-004` | Accepts settlement outcomes and leg-level handling. |
| Valuation rules and business acceptance | `STK-005` | Accepts valuation outcomes; does not approve accounting treatment. |
| Accounting, NAV-impact rules, and business acceptance | `STK-006` | Accepts accounting and NAV outcomes. |
| Reconciliation rules, tolerances, and business acceptance | `STK-007` | Accepts reconciliation outcomes and control evidence. |
| Control design and residual-risk treatment | `STK-008`, `STK-001` | `STK-008` reviews and endorses; `STK-001` accepts or escalates residual business risk. |
| Reference-data definitions and mappings | `STK-009` | Limited to owned reference-data domains and change controls. |
| Integration Layer design and technical feasibility | `STK-010` | Owns implementation-level `SYS-INT` design within approved requirements and architecture constraints. |
| Changes and release readiness for each participating system | `STK-011` | Each owner approves only the owned components interface. |
| Operational support readiness | `STK-012` | Confirms support model, monitoring, runbooks, and recovery readiness. |
| QA approach, coverage, and QA exit status | `STK-013` | Provides evidence and a recommendation; does not provide business UAT acceptance or final go/no-go. |
| Security requirements, exceptions, and security-control evidence | `STK-014` | Limited to the security and technology-risk domain. |

## 9. Engagement and involvement plan

| Stakeholder ID | Analysis and design involvement | Validation and acceptance involvement | Indicative engagement method |
|---|---|---|---|
| `STK-001` | Confirm objectives, scope, priorities, constraints, benefits, and escalation route. | Review unresolved risks and make milestone or final go/no-go decisions. | Steering review and decision log. |
| `STK-002` | Define source fields, event semantics, versioning, resend behaviour, and source constraints. | Review source mappings, lifecycle rules, and TBS-related acceptance evidence. | Data and lifecycle workshops; asynchronous specification review. |
| `STK-003` | Describe current operations, exceptions, manual corrections, cut-offs, and target ownership. | Review target process and execute or accept Trade Operations UAT. | Process workshops, playback sessions, and UAT. |
| `STK-004` | Define settlement data, leg statuses, cut-offs, failure scenarios, and correction paths. | Review settlement rules and execute or accept settlement UAT. | Settlement workshops, and scenario walkthroughs. |
| `STK-005` | Define valuation eligibility, required attributes, cut-offs, and quality controls. | Review valuation mappings and execute or accept Valuation UAT. | Valuation and control workshops. |
| `STK-006` | Define accounting treatment, posting expectations, NAV cut-offs, and blocking criteria. | Review accounting outputs and execute or accept Fund Accounting and NAV UAT. | Accounting-rule workshops and output review. |
| `STK-007` | Define populations, matching keys, tolerances, break classes, and ownership. | Review reconciliation results, control evidence, and reconciliation UAT. | Rule workshops and exception playback. |
| `STK-008` | Challenge risk assesment, control design, evidence escalation, and residual-risk treatment. | Review control coverage and unresolved residual risk at decision gates. | Control-design review and risk checkpoint. |
| `STK-009` | Define authoritative values, mappings, effective dating, ownership, and maintenance procedures. | Review reference-data validations, mappings, and negative scenarios. | Data workshops and mapping review. |
| `STK-010` | Design interfaces, canonical handling, errors, retries, idempotency, logging, and observability. | Support technical walkthroughs, defect analysis, and implementation verification. | Continuous delivery-team collaboration and technical review. |
| `STK-011` | Confirm system constraints, interface compability, capacity, dependencies, and release calendars. | Confirm owned-component readiness and approve required interface changes. | Interface reviews and release-readiness checkpoints. |
| `STK-012` | Define support ownership, monitoring, alerts, runbooks, recovery, and escalation needs. | Review failure scenarios and confirm operational support readiness. | Supportability workshop and operational-readiness review. |
| `STK-013` | Challenge testability, design coverage, test data, environments, traceability, and defect process. | Execute or coordinate QA, confirm exit status, and provide readiness evidence. | Three-amigos refinement, test reviews, and defect triage. |
| `STK-014` | Define security constraints for authentication, authorisation, secrets, logging, and access. | Review security requirements , exceptions, and relevant control evidence. | Security design review and exception review. |

## 10. Cross-functional conflict scenarios

| Conflict ID | Potential conflict | Required resolution |
|---|---|---|
| `STK-CFL-001` | Operations requests full lifecycle automation while Engineering identifies delivery or dependency constraints. | `STK-010` provides feasibility and options, affected process owners assess impact; `STK-001` decides scope and priority. |
| `STK-CFL-002` | Automated retries reduce manual work but may conceal repeated failures from Operations or Support. | `STK-010` and `STK-012` propose observable retry behaviour; `STK-003` and `STK-008` review exception visibility and control evidence. |
| `STK-CFL-003` | A common canonical format conflicts with a participating system's supported identifier or schema. | `STK-009` confirms business meaning and mapping; `STK-010` proposes the integration design; the affected `STK-011` owner approves the component change. |
| `STK-CFL-004` | A release meets technical QA exit criteria but has unresolved business or control concerns. | `STK-013` reports evidence; affected UAT stakeholders and `STK-008` provide their position; `STK-001` makes the final go/no-go decision. |
| `STK-CFL-005` | Settlement, valuation, accounting, and NAV teams require different processing cut-offs. | Affected process owners define minimum business constraints; `STK-010` and `STK-011` assess technical feasibility; `STK-001` resolved unresolved priority conflicts. |
| `STK-CFL-006` | Security controls or interface constraints reduce performance or increase delivery effots. | `STK-014` defines mandatory controls; `STK-010` and `STK-011` assess options, and `STK-001` decides non-mandatory tade-offs or escalate exceptions. |

## 11. Open questions

There questions are intentionally retained because they would require stakeholder elicitation in a real organisation. They do not block the continuation of the case study udner the assumsptions in Section 5.

| Open question ID | Open question | Proposed owner | Impacted area |
|---|---|---|---|
| `STK-OQ-001` | Are the Business Owner and Project Sponsor separate roles in the target organisation? | `STK-001` | Governance and escalation |
| `STK-OQ-002` | Are Front Office representation, source-data ownership, and TBS product ownership assigned to one group? | `STK-001`, `STK-002` | Source requirements and approvals |
| `STK-OQ-003` | Which team owns fund, account, counterparty, currency-pair, calendar, and cross-system mapping domains? | `STK-009` | Reference-data governance |
| `STK-OQ-004` | Is QA an independent delivery role, or are some system-test activities performed by operational teams? | `STK-001`, `STK-013` | Test governance and segregation responsibilites |
| `STK-OQ-005` | Which governance body may formally accept residual operational or technology risk? | `STK-001`, `STK-008`, `STK-014` | Risk acceptance |
| `STK-OQ-006` | What are the confirmed release calendars, maintenance windows, capacity limits, and operational cut-offs for each participating system? | `STK-004`, `STK-005`, `STK-006`, `STK-011` | Architecture, NFRs, and release planning |
| `STK-OQ-007` | Which stakeholders have authority to block release for unresolved UAT, control, security, or support-readiness findings? | `STK-001` | Go/no-go governance |

## 12. Risks arising from stakeholder management

| Risk ID | Stakeholder-management risk | Potential impact | Initial mitigation |
|---|---|---|---|
| `STK-RSK-001` | A critical process, data, or system owner is identified too late. | Rework, delayed decisions, incomplete requirements, or invalid assumptions. | Validate the register at project initiation and at each major scope change. |
| `STK-RSK-002` | Decision rights are unclear or overlap. | Conflicting approvals, delayes escalation, or unowned decisions. | Maintain the decision-right boundaries and decision log. |
| `STK-RSK-003` | An operational SME is unavailable during design or UAT. | Missing scenarios, weak acceptance evidence, or delayed testing. | Nominate primary and backup SMEs and agree review windows early. |
| `STK-RSK-004` | Technical design proceeds without sufficient business or data-owner input. | Technically valid interfaces that misinterpret trade or accounting meaning. | Require cross-functional review of lifecycle semantics, mapping, and acceptance criteria. |
| `STK-RSK-005` | A stakeholder is informed when consultation or approval was required. | Late objections, control gaps, or release delay. | Use the engagement plan and record material reviews and decisions. |
| `STK-RSK-006` | One team performs multiple roles without clear responsibility boundaries. | Self-approval, missed independent challenge, or unclear accountability. | Record the role bing performed and define separate review or approval where required. |

## 13. Decisions recorded by this document

| Decision ID | Decision |
|---|---|
| `STK-DEC-001` | Stakeholders are modelled as role-based functional groups rather than named individuals. |
| `STK-DEC-002` | Business UAT acceptance remains with the relevant operational stakeholder; QA confirms test completion and provides readiness evidence. |
| `STK-DEC-003` | Domain approval, component release readiness, and final project go/no-go are treated as separate decisions. |
| `STK-DEC-004` | Information Security/Technology Risk is included as a supporting consulted stakeholder, without claiming production-grade security implementation. |
| `STK-DEC-005` | Open organisational questions are retained transparently rather than presented as verified facts. |

## 14. Downstream traceability

Stakeholder IDs in this document will be reused in later artefacts to identify requirement sources, reviewers, decision owners, UAT participants, and control owners.

Expected downstream links include:

- business and system requirements;
- current-state and target-state process models;
- lifecycle state and sequence models;
- canonical data dictionary and source-to-target mapping;
- business rules and validation matrix;
- API functional and OpenAPI specifications;
- error catalogue and non-functional requirements;
- risk and controls matrix;
- UAT plan, test cases, and defect governance;
- requirements traceability matrix and Jira-style backlog.

The traceability matrix will reference `STK-001` through `STK-014` rather than introducing alternative stakeholder names.