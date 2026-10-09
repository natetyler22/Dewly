# Dewly — Initial Requirements Backlog

**Version:** 1.0  
**Date:** October 9, 2026  
**Status:** Proposed; awaiting stakeholder validation  
**Purpose:** Initial requirements elicitation for an unlimited-capital AI skincare platform.

## 1. Elicitation context and objectives

The student proposes using facial photographs plus lifestyle and diet information to provide personalized skincare information, including serums and potential lifestyle changes. This AI-assisted initial pass translates that vision into testable software requirements while avoiding promises of perfect accuracy or medical conclusions unsupported by photographs.

Objectives:
- O1: Make personalized skincare education understandable and actionable.
- O2: Reduce unsuitable product recommendations and recognize situations requiring professional care.
- O3: Let users track observations and self-reported outcomes without asserting causation.
- O4: Protect sensitive information and user autonomy.
- O5: Establish evidence, inclusivity, and operational quality before release.

Method: analyze the supplied concept, identify stakeholders and user journeys, elicit functional and quality requirements, identify dependencies, and record unresolved decisions. No stakeholder interviews or clinical validation were performed. All requirements are proposals; all acceptance criteria are future verification conditions, not achieved results.

## 2. Stakeholders

| Code | Stakeholder | Interests and elicitation needs |
| --- | --- | --- |
| U | Adult end users | Useful information, affordability, understandable uncertainty, privacy, accessible capture |
| C | Dermatologists and clinical safety team | Appropriate scope, evidence, escalation, contraindications, safe wording |
| N | Registered dietitians | Responsible nutrition education, referral boundaries, eating-disorder-sensitive content |
| P | Privacy, legal, and security teams | Consent, jurisdiction, retention, security, regulatory classification |
| E | Engineers, ML researchers, and QA | Testable behavior, data quality, model validation, deployment and recovery |
| A | Accessibility and inclusion representatives | Disability access, skin-tone representation, language and device coverage |
| B | Product, operations, and support teams | Prioritization, support, adoption, service reliability, commercial transparency |
| R | Research participants and ethics reviewers | Voluntary participation, data provenance, study governance |
| I | Instructor / academic reviewer | Clear requirements metadata, dependencies, and an accessible repository |

## 3. Assumptions and decisions to validate

| ID | Assumption | Implication / validation owner |
| --- | --- | --- |
| A1 | Funding is unlimited, but evidence, time, recruitment, staffing, and scientific uncertainty remain constraints. | Sequence work by risk and dependency, not budget alone; B/C/E. |
| A2 | The first pilot serves adults 18+ in one jurisdiction. | No minors or global release until separately approved; P/B. |
| A3 | Initial intended use is cosmetic skincare education, not autonomous diagnosis or treatment. | Every feature and marketing claim needs intended-use review; C/P. |
| A4 | Lifestyle and diet information is optional, self-reported, and may be incomplete. | Missing context must reduce personalization rather than trigger invented facts; U/N/E. |
| A5 | Photo analysis is limited to clinician-approved visible features validated for supported populations and devices. | Unsupported cases abstain; C/A/E. |
| A6 | Photos are sensitive, and training/research reuse is not necessary to use the core service. | Separate optional consent and data-use controls; P/R. |
| A7 | Numerical service targets below are provisional and subject to pilot findings. | Capacity and operational sign-off required; E/B. |
| A8 | Product databases and professional integrations can be licensed, but must be verified. | No assumed universal catalog coverage or immediate clinician access; C/B. |

## 4. Metadata conventions

- **IDs:** D-001 through D-056; stable across revisions.
- **Types:** FR = functional; NFR = nonfunctional quality; SAF = clinical/safety constraint; PRIV = privacy/security; DATA = data/ML governance; BUS = business/process.
- **Priorities:** Must = release blocker for the feature's release; Should = important expansion; Could = optional later capability. Priority is not a delivery date.
- **Releases:** P = pilot; E = expansion; R = research. A Must in E or R blocks that capability, not the pilot.
- **Dependencies:** prerequisite requirements for acceptance/release, not necessarily a serial implementation schedule. “None” means no earlier backlog prerequisite.
- **Notes:** stakeholder codes plus assumptions or unanswered elicitation concerns.
- Every acceptance criterion is mandatory for the requirement to be accepted. Clinically approved test suites, thresholds, and feature allowlists must be established before relevant capabilities are enabled.

## 5. Requirements

### Governance, eligibility, and onboarding

| ID | Type | Priority / release | Requirement | Dependencies | Acceptance criteria | Stakeholder / assumption notes |
| --- | --- | --- | --- | --- | --- | --- |
| D-001 | BUS | Must / P | Define intended use, excluded claims, launch jurisdiction, and regulatory responsibilities. | None | C and P approve a written scope and jurisdiction-specific review covering image analysis, referral, nutrition, and marketing; unapproved capabilities remain disabled. | C/P/B; A2–A3. Education wording alone does not establish regulatory status. |
| D-002 | SAF | Must / P | Limit the pilot to eligible adults. | D-001 | Under-18 self-reported users cannot submit photos or receive personalized outputs; missing eligibility prevents enrollment; eligibility limitations are explained without collecting unnecessary identity documents. | U/P; A2. Age-assurance approach remains to be decided. |
| D-003 | FR | Must / P | Provide account creation, sign-in, recovery, and sign-out. | D-002, D-041 | Valid users can create and recover accounts; invalid/expired recovery links fail; sign-out invalidates the active session; errors do not disclose account existence. | U/E/P. |
| D-004 | PRIV | Must / P | Obtain understandable, purpose-specific consent before collecting photos or health context. | D-001, D-002 | Capture and upload remain blocked until required consent is recorded with version, timestamp, and purposes; optional training consent defaults off; declining optional purposes preserves core access. | U/P; A6. |
| D-005 | FR | Must / P | Collect editable goals and relevant self-reported context. | D-003, D-004 | Users can record concerns, routine, sensitivities, current products, and relevant optional context; skip optional fields; distinguish unknown from negative answers; correct information and regenerate a plan. | U/C/N; A4. Pregnancy/medication context requires sensitive handling. |
| D-006 | FR | Should / E | Collect optional lifestyle and diet preferences without requiring detailed food logs. | D-005, D-029 | Users can skip, edit, or remove diet, sleep, and lifestyle inputs; removal stops their use in new personalization; no deficiency or causal diagnosis is inferred from entries. | U/N; A4. |
| D-007 | FR | Must / P | Support account and notification preferences. | D-003, D-004 | Users can update preferences; marketing defaults off; notification changes take effect before the next send; optional notifications do not gate analysis. | U/P/B. |

### Photography and image handling

| ID | Type | Priority / release | Requirement | Dependencies | Acceptance criteria | Stakeholder / assumption notes |
| --- | --- | --- | --- | --- | --- | --- |
| D-008 | FR | Must / P | Guide capture of supported facial views. | D-004, D-046 | Users receive instructions for lighting, framing, and filters; preview and retake before submission; alternative accessible instructions are available; capture requests only supported views. | U/A/C; A5. |
| D-009 | FR | Must / P | Reject images that fail approved quality checks. | D-008, D-017 | Approved fixtures for blur, poor lighting, occlusion, unsupported content, and absent/multiple faces produce specific retake messages; rejected images produce no skin observations. | U/C/E/A; A5. Check performance across skin tones. |
| D-010 | PRIV | Must / P | Validate uploads and remove unnecessary metadata. | D-004, D-041 | Allowlisted image formats and configured size limits are enforced; disguised or malformed files are rejected; GPS and unnecessary EXIF are removed before storage and analysis; original timestamps are not exposed. | P/E. |
| D-011 | FR | Must / P | Handle interrupted upload and analysis without duplicate records. | D-010 | Interrupted requests can be retried; retries create at most one analysis record; users see progress, failure, or completion; partial uploads are cleaned up within 24 hours. | U/E; A7. |
| D-012 | PRIV | Must / P | Prevent photo use for identity recognition and limit background exposure. | D-010 | No facial-identity matching or cross-user face search is enabled; unnecessary background is excluded where feasible; a non-user/multiple-person image cannot proceed through the quality gate. | U/P/E; A6. Crop is data minimization, not anonymization. |

### Analysis, uncertainty, and model evidence

| ID | Type | Priority / release | Requirement | Dependencies | Acceptance criteria | Stakeholder / assumption notes |
| --- | --- | --- | --- | --- | --- | --- |
| D-013 | SAF | Must / P | Establish a clinician-approved observation allowlist and prohibited inferences. | D-001 | C signs off supported visible features and wording; diagnosis, cancer screening, nutrient-deficiency inference, and guaranteed outcomes are excluded from the pilot; feature changes require review. | C/P/E; A3/A5. |
| D-014 | FR | Must / P | Produce observations only for supported, validated features. | D-009, D-013, D-018 | Valid images return only allowlisted observations; each report identifies model version and assessment date; unsupported outputs are blocked in a clinician-reviewed test suite. | U/C/E. |
| D-015 | SAF | Must / P | Explain uncertainty and abstain from unreliable assessment. | D-014 | Every observation includes an understandable reliability explanation; approved low-confidence and out-of-distribution fixtures return “cannot assess”; abstention does not become a fabricated normal result. | U/C/A/E. |
| D-016 | DATA | Must / P | Use lawfully sourced, consented, documented development datasets. | D-001, D-004 | Dataset records contain source, permitted purposes, consent/license basis, limitations, and retention policy; unapproved data cannot enter training; audit sample finds complete provenance. | P/E/R; A6. |
| D-017 | DATA | Must / P | Define representative validation datasets and evaluation protocols. | D-013, D-016 | C/E/A approve separate training and held-out evaluation splits with no person overlap, intended population/device coverage, label-review methods, and sample-size rationale; limitations are recorded. | C/A/E; A5. Includes quality-gate evaluation. |
| D-018 | DATA | Must / P | Pass prespecified overall and subgroup validation gates. | D-017 | Before evaluation, C/E/A record feature-specific accuracy, calibration, abstention, and subgroup error thresholds; held-out results and uncertainty intervals satisfy every approved gate; unmet gates disable the affected capability. | C/A/E. No invented “99% accuracy” claim. |
| D-019 | DATA | Must / P | Monitor model performance and support safe withdrawal. | D-018, D-043 | Monitoring detects approved drift signals; an exercised incident drill disables a model and falls back to education/referral within 30 minutes; retired versions cannot generate new reports. | E/C/A; A7. |
| D-020 | FR | Must / P | Let users report incorrect observations and request support. | D-014, D-043 | A report can reference an analysis without duplicating photos into general support logs; users receive a tracking reference; authorized reviewers can categorize and resolve feedback. | U/C/B/P. |

### Skincare routines and product guidance

| ID | Type | Priority / release | Requirement | Dependencies | Acceptance criteria | Stakeholder / assumption notes |
| --- | --- | --- | --- | --- | --- | --- |
| D-021 | DATA | Must / P | Maintain a reviewed ingredient and product knowledge base. | D-001 | Entries include ingredient list, evidence reference, limitations, review date, and reviewer; outdated or unverified entries cannot support recommendations; version history is retained. | C/E/B; A8. |
| D-022 | SAF | Must / P | Check reported allergies, sensitivities, existing products, and contraindications before recommending products. | D-005, D-021 | All clinician-approved conflict fixtures block or qualify the affected suggestion; missing essential context causes a clarification or abstention; no allergy-free guarantee is made. | C/U; A4. |
| D-023 | FR | Must / P | Build an understandable personalized cosmetic routine. | D-015, D-022 | Eligible cases receive ordered morning/evening steps and appropriate serum/product categories; each step explains rationale and evidence limitations; low-confidence cases receive only approved general education. | U/C; A3/A5. |
| D-024 | SAF | Must / P | Explain cautious introduction and adverse-reaction handling. | D-023, D-028 | New-product plans display clinician-approved introduction and product-testing education; approved reported-reaction fixtures suspend affected suggestions and show appropriate escalation instructions. | U/C. Avoid guaranteed tolerance. |
| D-025 | FR | Should / E | Respect budget, availability, and preferences in product alternatives. | D-021, D-023 | A supported region and budget return eligible alternatives or a clear no-match result; displayed price/availability has source and timestamp; unavailable items are not represented as in stock. | U/B; A8. |
| D-026 | BUS | Must / P | Disclose commercial influence and separate ranking from sponsorship. | D-021, D-023 | Sponsored/affiliate placements are labeled; safety eligibility cannot be overridden by payment; ranking documentation and audit demonstrate sponsored status does not alter clinical suitability. | U/C/B/P. |
| D-027 | FR | Should / E | Let users edit routines and report adherence. | D-023, D-022 | Users can skip or replace a step, record use, and undo edits; a replacement reruns safety checks; user changes remain distinguishable from Dewly suggestions. | U/C. |

### Medical safety and nutrition boundaries

| ID | Type | Priority / release | Requirement | Dependencies | Acceptance criteria | Stakeholder / assumption notes |
| --- | --- | --- | --- | --- | --- | --- |
| D-028 | SAF | Must / P | Provide clinician-approved escalation based on user-reported symptoms and scope limitations. | D-001, D-013 | A signed-off symptom scenario suite shows appropriate routine, urgent, or emergency guidance; severe-symptom paths interrupt routine advice; an unremarkable image never overrides reported symptoms or implies clearance. | U/C/P. No autonomous disease-screening claim. |
| D-029 | SAF | Must / E | Establish nutrition content boundaries and qualified review. | D-001 | N/C approve a policy excluding deficiency diagnosis from photos, unsupported elimination diets, treatment supplements, and guaranteed skin outcomes; every published nutrition module has a reviewer and evidence record. | N/C/U; A3/A4. |
| D-030 | FR | Should / E | Offer optional general nutrition and lifestyle education. | D-006, D-029 | Users can view approved content based on stated preferences; output acknowledges uncertainty about skin effects; medical, pregnancy, allergy, or eating-disorder-sensitive scenarios use approved limitations/referral paths. | U/N/C. |
| D-031 | FR | Should / E | Facilitate user-initiated access to qualified professionals. | D-028, D-037, D-041 | Provider qualifications and service jurisdictions are verified; sharing requires consent for specified data; scheduling failure is explained; the service never promises immediate availability. | U/C/N/P; A8. |
| D-032 | SAF | Must / P | Keep generated text within approved evidence and safety boundaries. | D-013, D-021, D-028 | Reviewed adversarial prompts for diagnosis, prescriptions, unsafe routines, fabricated sources, and embedded image instructions produce approved refusals or referrals; failing the safety suite blocks release. | C/E/P. |

### Tracking, engagement, and professional workflow

| ID | Type | Priority / release | Requirement | Dependencies | Acceptance criteria | Stakeholder / assumption notes |
| --- | --- | --- | --- | --- | --- | --- |
| D-033 | FR | Should / E | Show comparable longitudinal photo and observation history. | D-014, D-038 | Users see dates and analysis versions; incompatible lighting/model conditions trigger comparison warnings; deleted photos disappear; interface does not label change as treatment causation. | U/C/E. |
| D-034 | FR | Should / E | Record user-reported outcomes and adverse experiences. | D-027, D-028 | Users can record benefit, irritation, and no change; severe-reaction entries trigger approved guidance; stored records distinguish self-reports from model observations. | U/C. |
| D-035 | FR | Could / E | Provide opt-in reminders for routines and follow-up. | D-007, D-023 | Reminders respect timezone and quiet hours; unsubscribe stops optional reminders; lock-screen content does not reveal skin concerns or photos by default. | U/P/B. |
| D-036 | FR | Must / E | Restrict professional review to authorized, consented cases. | D-031, D-042 | Authorized professionals access only assigned cases and consented fields; they can document advice with identity and date; revoked sharing blocks further access; every access is audited. | C/N/P/U. |

### Privacy, security, and data rights

| ID | Type | Priority / release | Requirement | Dependencies | Acceptance criteria | Stakeholder / assumption notes |
| --- | --- | --- | --- | --- | --- | --- |
| D-037 | PRIV | Must / P | Provide granular purpose and sharing controls. | D-004 | Users can view and revoke optional sharing, research, and training consent; revocation stops new use before the next processing job; its implications for prior studies/trained models are explained truthfully. | U/P/R; A6. |
| D-038 | PRIV | Must / P | Enforce retention limits and verified deletion. | D-001, D-004 | P approves a field-level schedule before collection; proposed default raw-photo retention is 30 days unless user elects history; active-data deletion completes within 30 days and backup expiry within 90 days; exceptions are disclosed and approved. | U/P/E; A7. Exact periods require jurisdiction review. |
| D-039 | PRIV | Must / P | Export user data in usable formats. | D-003, D-041 | Authenticated users receive their available photos, profile, reports, and consent history in documented formats within 7 days; export links expire within 24 hours and cannot expose another account. | U/P/E; A7. |
| D-040 | PRIV | Must / P | Encrypt sensitive data in transit and at rest with controlled keys. | D-001 | Network tests reject plaintext access and obsolete protocols; storage and backups use approved encryption; key access is restricted and logged; rotation and recovery are exercised. | P/E. |
| D-041 | PRIV | Must / P | Secure authentication, sessions, and privileged actions. | D-040 | Privileged accounts require MFA; sensitive actions require recent authentication; session expiry, rate limits, recovery, and cross-account access tests pass; secrets are absent from client bundles and repository. | P/E. |
| D-042 | PRIV | Must / P | Enforce least privilege and tenant isolation. | D-041 | A documented role matrix is implemented; tests deny cross-user and unassigned clinician access; administrative access requires a recorded purpose and is periodically reviewed. | P/E/C. |
| D-043 | PRIV | Must / P | Maintain privacy-conscious audit logs and incident response. | D-042 | Logs capture security, consent, access, and model changes without raw photos or unnecessary health text; a breach tabletop verifies detection, containment, and jurisdiction-approved notification responsibilities. | P/E/B. |
| D-044 | PRIV | Must / P | Prohibit sensitive-data sales and unapproved third-party tracking. | D-001, D-037 | Network and vendor audits find no health/photo data transmitted to advertising trackers; vendors have approved processing terms; explicit optional consent is required for any separately approved secondary use. | U/P/B; A6. |
| D-045 | PRIV | Must / P | Complete security review before release. | D-040, D-041, D-042, D-043, D-044 | Threat model covers upload, APIs, model inputs, vendors, and privileged access; independent testing has no unresolved critical/high findings; remediation and risk owners are documented. | P/E. |

### Accessibility and operations

| ID | Type | Priority / release | Requirement | Dependencies | Acceptance criteria | Stakeholder / assumption notes |
| --- | --- | --- | --- | --- | --- | --- |
| D-046 | NFR | Must / P | Make core journeys accessible against WCAG 2.2 AA. | D-001 | Manual and automated audits cover keyboard use, screen readers, contrast, non-color confidence cues, form errors, and camera alternatives; no unresolved AA failures in core journeys. | A/U/E. Final conformance scope requires audit. |
| D-047 | NFR | Must / P | Meet approved performance targets under representative pilot load. | D-011, D-014 | Under a documented 100-concurrent-analysis workload, 95% of valid analyses finish within 30 seconds after upload and 95% of normal API requests within 2 seconds; timeout paths provide clear retry status. | U/E; A7. Hardware/network profile must be specified. |
| D-048 | NFR | Must / P | Establish reliability, backup, and recovery capabilities. | D-040, D-043 | Proposed monthly availability is 99.9%; a restore drill meets recovery time of 4 hours and recovery point of 24 hours; deleted-data records prevent restoring expired/deleted content into service. | E/B/P; A7. |
| D-049 | NFR | Should / E | Support an explicitly tested browser/device matrix. | D-008, D-046 | Core capture, consent, and report journeys pass on documented supported mobile and desktop platforms; unsupported platforms receive an explanation and usable alternative where feasible. | U/A/E; A5. |
| D-050 | FR | Should / E | Localize approved content and regional product guidance. | D-001, D-025, D-032 | Each enabled locale passes qualified language and clinical review; regional providers/products and safety contacts are validated; unsupported locales do not receive assumed local guidance. | U/A/C/P. |
| D-051 | BUS | Must / P | Offer support and measure privacy-preserving product outcomes. | D-020, D-043, D-044 | Users can submit support requests without photos; approved aggregate metrics cover completion, abstention, adverse reports, and accessibility issues; dashboards suppress groups below 20 users. | B/U/P; A7. Suppression is not an anonymity guarantee. |
| D-052 | DATA | Must / P | Version and approve model, knowledge-base, and safety-policy changes. | D-018, D-019, D-032 | Each release records model/data/policy versions, review sign-offs, validation evidence, and rollback instructions; unapproved versions cannot deploy; reports are traceable to their released version. | E/C/P/A. |

### Research and future scope

| ID | Type | Priority / release | Requirement | Dependencies | Acceptance criteria | Stakeholder / assumption notes |
| --- | --- | --- | --- | --- | --- | --- |
| D-053 | DATA | Must / R | Govern optional research participation separately from the service. | D-016, D-037, D-038, D-045 | Appropriate ethics review approves each study; participation is optional with study-specific consent; withdrawal and retention terms are explained; service access remains available to nonparticipants. | R/P/C/U; A6. |
| D-054 | DATA | Could / R | Run prospective studies of usefulness and real-world safety. | D-053, D-052 | Study protocols prespecify endpoints, recruitment, comparator, sample-size rationale, and adverse-event review; reports include limitations and subgroup findings; no effectiveness claim precedes review. | R/C/A/E. |
| D-055 | BUS | Must / P | Maintain requirements traceability and change control. | D-001 | Every release maps applicable requirement IDs to test or review evidence and an owner; additions retain IDs and rationale; stakeholder approval is recorded for changed scope or acceptance thresholds. | I/B/E/C/P. |
| D-056 | SAF | Must / E | Gate any future medical functionality through a separate approval pathway. | D-001, D-054, D-055 | Diagnosis, treatment, prescriptions, and regulated clinical decisions remain disabled until intended-use, clinical evidence, professional responsibility, and applicable regulatory approvals are documented. | C/P/R/B; A3. No automatic progression from research to medical service. |

## 6. Dependency and release interpretation

The tables define all prerequisites; dependency IDs form an acyclic graph. Important paths:
- Governance and consent: D-001 → D-002/D-004 → eligible, consented capture.
- Validation: D-013/D-016 → D-017 → D-018 → D-014 → D-015.
- Routine safety: D-021/D-005 → D-022; D-015/D-022 → D-023 → D-024.
- Nutrition: D-001 → D-029; D-005/D-029 → D-006; D-006/D-029 → D-030.
- Research: privacy and dataset controls → D-053 → D-054 → D-056.

Foundational security, quality, and governance work can run concurrently. A feature remains disabled until its own requirements and all dependencies are accepted.

Pilot release requires every **Must / P** item to pass, clinical and privacy sign-off, validated supported populations, accessibility review, and a tested fallback. Expansion features may be deferred; their **Must / E** controls must pass before enabling them. Research is a separate consent and governance track.

## 7. Priority rationale and tradeoffs

Must requirements prevent unsafe, unconsented, misleading, inaccessible, or unreviewable operation. Should requirements increase usefulness and breadth after those foundations. Could requirements add engagement or research value without being necessary for the educational pilot.

Key tradeoffs:
- More photographs can improve comparability but increase burden and privacy exposure; require only validated necessary views.
- More personalization needs more sensitive information; optional inputs must remain optional.
- Broad model coverage is desirable, but unsupported populations or features must remain outside enabled scope until validated.
- Commercial partnerships may improve availability; suitability and evidence must remain independent of sponsorship.
- Historical tracking conflicts with short retention; users must explicitly choose history with transparent deletion controls.

## 8. Open elicitation questions

| Question | Stakeholders | Affected requirements |
| --- | --- | --- |
| Which jurisdiction launches first, and which features change legal classification? | P/C/B | D-001, D-028, D-056 |
| Which observable cosmetic features are supportable, and how should users describe their goals? | U/C/A/E | D-005, D-013–D-018 |
| What thresholds, calibration measures, and subgroup sample sizes establish acceptable performance? | C/A/E | D-017–D-019 |
| What age assurance and handling of accidental minor enrollment are appropriate? | P/U | D-002, D-038 |
| Which health-context questions are necessary, and when should missing answers prevent suggestions? | U/C/N/P | D-005, D-022, D-030 |
| How should clinical escalation work without implying automated screening or guaranteed access? | U/C/P/B | D-028, D-031 |
| Which retention periods and research withdrawal obligations apply? | U/P/R | D-037–D-039, D-053 |
| Which devices, accessibility needs, languages, and budget ranges must the pilot support? | U/A/E/B | D-025, D-046–D-050 |
| What business model avoids conflicts of interest and excludes sensitive-data sales? | U/P/B/C | D-026, D-044 |
| Which validated user outcomes define success without implying causation or body-image harm? | U/C/N/R/B | D-033, D-034, D-051, D-054 |

## 9. Definition of ready and done

**Ready:** stakeholder owner identified; assumption and jurisdiction implications reviewed; dependencies understood; acceptance criteria and any clinical thresholds approved; necessary test data available with lawful provenance.

**Done:** applicable acceptance criteria verified; results linked to IDs; dependencies accepted; privacy/security and clinical reviews completed; accessibility evidence recorded where relevant; limitations and user-facing content reviewed; release owner signs off.

For this homework, completion means a readable README and comprehensive initial backlog committed to an accessible GitHub repository. It does not mean the proposed application requirements are implemented or clinically proven.

## 10. Background sources

These sources prompted questions and safeguards rather than proving the proposed platform safe or compliant:
- [AAD: How to test skin care products](https://www.aad.org/public/everyday-care/skin-care-secrets/prevent-skin-problems/test-skin-care-products/) — supports elicitation of cautious product introduction and adverse-reaction guidance.
- [FDA: General Wellness—Policy for Low Risk Devices](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/general-wellness-policy-low-risk-devices) — informs intended-use and regulatory review questions.
- [FTC: Complying with the Health Breach Notification Rule](https://www.ftc.gov/business-guidance/resources/complying-ftcs-health-breach-notification-rule-0) — informs privacy and incident-response review questions.

No jurisdiction-specific compliance conclusion is asserted. Review these sources and applicable requirements again before implementation.
