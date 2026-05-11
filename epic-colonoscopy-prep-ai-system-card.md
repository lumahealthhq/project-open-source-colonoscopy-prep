# AI System Card
## Project Open Source Colonoscopy Prep Agent
**Luma Health · Open Source Workflow · Epic EHR Integration**

| Field | Value |
|-------|-------|
| Version | 1.0 (Open Source Release) |
| Date | April 2026 |
| Status | Production-Ready |
| License | Apache 2.0 |

---

## 1. System Overview

The Luma Health Project Open Source Colonoscopy Prep Agent is an AI-powered administrative workflow system that automates patient outreach and preparation communications in the days preceding a scheduled colonoscopy. It is distributed as an open-source workflow for any health system or ambulatory surgery center (ASC) running Epic EHR, and is available free of charge for 90 days at new Luma + Epic deployment sites with a 15-day go-live commitment.

| Field | Value |
|-------|-------|
| System Name | Colonoscopy Prep Coordination Agent |
| Developer | Luma Health |
| Version | 1.0 (Open Source Release) |
| Release Date | April 2026 |
| EHR Integration | Epic (native) |
| Distribution | Open source — full playbook, call scripts, and configuration templates published on GitHub |
| Deployment Model | Any health system or ASC running Epic EHR |
| Go-Live Timeline | 15-day standard go-live; 90-day free onboarding period for new Luma + Epic sites |
| Languages | English and Spanish |
| Underlying AI Model | OpenAI GPT-4 and GPT-5 family (voice + language understanding, zero data retention) |
| System Type | Administrative workflow automation — not a clinical decision support system |

---

## 2. Intended Use & Scope

### 2.1 Primary Purpose

The agent performs pre-procedure administrative coordination on behalf of health system staff. It contacts patients scheduled for colonoscopy procedures in the days leading up to their appointment, delivers preparation instructions sourced from the patient's Epic chart, answers common patient questions, and confirms readiness. It is explicitly **not a clinical decision support system** and does not make, recommend, or alter any clinical decisions.

### 2.2 Target Users

The system serves three distinct populations:

- **Patients** scheduled for colonoscopy procedures who receive outbound calls, respond to inbound calls, or engage via SMS.
- **Health system and ASC staff** (GI nurses, schedulers, administrative coordinators) whose manual pre-call burden is reduced by the agent.
- **Epic EHR administrators and IT teams** responsible for workflow configuration and integration setup.

### 2.3 Communication Modalities

| Channel | Description |
|---------|-------------|
| Outbound phone calls | AI voice agent initiates calls to patients according to the site-configured prep schedule |
| Inbound call handling | Patients can call back and interact with the agent to ask questions or confirm prep completion |
| SMS / text messaging | Supplementary text-based reminders and links to prep instructions |

### 2.4 Workflow Touchpoints

Specific outreach timing (e.g., 7 days, 3 days, 1 day before procedure, day-of check-in) is configurable per deploying site. Luma publishes recommended templates on GitHub, but each health system defines its own schedule based on operational needs and patient population. Interaction data is written back into the Epic workflow at each touchpoint.

### 2.5 Out of Scope

The following are explicitly **not supported** by this system:

- Diagnose, treat, or provide clinical recommendations of any kind
- Modify, override, or create clinical orders in Epic
- Interpret symptoms or triage patients to clinical pathways
- Handle emergencies — any patient reporting a medical emergency is immediately instructed to call 911
- Replace physician or nursing judgment
- Operate outside the colonoscopy prep workflow (no other procedure types in this release)

---

## 3. Epic EHR Integration

The agent is embedded within the health system's Epic instance via Luma Health's native Epic integration layer. Luma acts as a sequencing harness that connects orders, scheduling data, results, and patient messaging into a single workflow driven by what is present in the patient's chart at the time of each interaction.

### 3.1 Data Read from Epic

- Scheduled procedure date, time, and location
- Patient demographics and preferred contact information
- Assigned prep protocol and dietary restriction instructions
- Referring and performing provider information
- Patient-preferred language (drives English vs. Spanish call routing)

### 3.2 Data Written Back to Epic

- Call attempt logs (timestamp, outcome: answered / no answer / opted out)
- Patient confirmation of prep instruction receipt
- Patient-reported prep completion status (same-day check-in)
- Escalation flags — cases routed to live staff are logged with reason
- SMS delivery and read receipts where available

### 3.3 Integration Boundaries

The system operates in a read-heavy, write-limited pattern. It never creates, modifies, or cancels orders, encounters, or clinical documentation. All write-back activity is limited to administrative workflow fields within Luma's integration scope. Epic's own access controls, audit logging, and role-based permissions govern all data access.

---

## 4. AI Model & Technical Architecture

### 4.1 Model Stack

| Component | Technology |
|-----------|------------|
| Language Model | OpenAI GPT-4 and GPT-5 family |
| Voice Interface | ElevenLabs and Deepgram speech-to-text and text-to-speech |
| Orchestration Layer | Luma Health Spark engine, integrated with Epic via HL7 FHIR and Epic APIs and Epic Reporting Workbench reporting |
| Conversation Scope | Constrained prompt design — agent operates within a defined topic boundary for colonoscopy prep |
| Context Source | Patient chart data retrieved from Epic at call initiation; agent does not retain cross-session memory |

### 4.2 Prompt Design & Topic Constraints

The agent is configured with a system prompt that restricts its responses to colonoscopy preparation topics. It is instructed not to speculate on clinical matters, not to discuss other conditions or medications beyond those relevant to prep, and to escalate any question it cannot confidently answer from its configured knowledge base. Configuration templates are published openly on GitHub to enable community review and improvement.

### 4.3 Human Escalation

Escalation to a live staff member is triggered in the following conditions:

- Patient asks a question the agent cannot answer within its defined scope
- Patient expresses distress, confusion, or dissatisfaction during the call
- Patient reports a symptom or medical concern
- Patient explicitly requests to speak with a person
- Agent confidence falls below the configured threshold for a given response

Escalated calls are flagged in Epic and routed to the health system's designated staff queue. The agent does not attempt to resolve clinical questions before escalating.

---

## 5. Performance & Success Metrics

| Outcome Metric | Definition | Measurement Source |
|----------------|------------|--------------------|
| Procedure Completion Rate | Percentage of scheduled colonoscopies that proceed as planned (no same-day cancellation due to inadequate prep or no-show) | Epic scheduling & case data |
| Adequate Prep Rate | Percentage of completed procedures where prep quality was rated adequate by the endoscopist | Procedure documentation in Epic |
| Staff Operational Burden | Reduction in manual pre-call time per procedure; measured against pre-deployment baseline | Staff time tracking / call logs |
| Patient Confidence Score | Patient-reported comfort and readiness at day-of check-in (captured via agent interaction) | Agent interaction logs |
| No-Show Rate | Percentage of scheduled cases where patient does not arrive | Epic scheduling data |
| Case Cancellation Rate | Same-day cancellations attributed to prep failure or patient unreadiness | Epic scheduling data |

Luma publishes benchmark ranges from existing deployments on GitHub. Individual health systems are encouraged to establish their own baselines prior to go-live and to track 30/60/90-day post-deployment comparisons.

---

## 6. Known Risks, Limitations & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Patient receives incorrect prep instructions due to stale chart data | Low | High | Agent reads from Epic at call initiation; health system responsible for order accuracy in Epic |
| Agent misunderstands patient speech (accented English, background noise) | Medium | Medium | Confidence thresholds trigger re-prompt or escalation; SMS fallback available |
| Patient interprets agent guidance as clinical advice | Medium | High | Agent explicitly identifies itself as an automated administrative assistant at call start; all clinical questions escalated |
| Escalation fails — live staff unavailable at time of transfer | Medium | Medium | Escalation flag written to Epic; staff follow-up queue with SLA configurable per site |
| Spanish-language nuance or regional variation mishandled | Medium | Medium | GPT-4 multilingual capability; site can review and customize scripts via open-source templates |
| Patient opts out of AI contact, misses critical prep instructions | Low | High | Opt-out patients flagged in Epic for manual staff outreach; opt-out honored immediately |
| Over-reliance on agent; staff reduce manual safety net prematurely | Low | High | Deployment playbook recommends maintaining staff oversight for first 90 days; escalation logging monitored |
| Data breach or unauthorized access to PHI in transit | Low | High | All data transmitted over TLS; Epic access governed by health system's BAA and access controls |

---

## 7. Equity, Accessibility & Patient Considerations

### 7.1 Language Access

The agent supports English and Spanish. Patient-preferred language is read from the Epic record and used to route the interaction to the appropriate language model.

### 7.2 Health Literacy & Plain Language

Call scripts are written at a recommended 6th–8th grade reading level and reviewed for health literacy alignment. Scripts are openly published and modifiable — health systems may adapt language for their specific patient populations. Patients who express confusion are prompted with simplified explanations before escalation is offered.

### 7.3 Patients Who Cannot or Prefer Not to Use AI Voice

- Patients may opt out of AI contact at any time during any interaction
- Hearing-impaired or deaf patients relying on SMS receive the same prep information via text; complex questions are flagged for staff follow-up
- Patients without reliable phone access should be identified at scheduling and handled via standard manual outreach protocols
- Opt-out and accessibility flags are written back to Epic and trigger staff outreach assignments

### 7.4 Vulnerable Populations

The agent is not designed to independently manage patients with complex comorbidities, recent hospital discharge, or cognitive impairment. Health systems are encouraged to establish clinical criteria for excluding specific patients from AI outreach and routing them to direct staff contact. Configuration guidance is included in the open-source playbook.

---

## 8. Privacy, Data Governance & Compliance

| Area | Details |
|------|---------|
| PHI Handling | All patient data is processed under the health system's existing Epic BAA and Luma Health's BAA; no PHI is retained by OpenAI for training purposes under Luma's enterprise Zero Data Retention agreement |
| Data in Transit | All communications encrypted via TLS 1.2 or higher |
| Data at Rest | Call logs and interaction records stored within Luma Health's HIPAA-compliant infrastructure protected with AES-256 encryption |
| HIPAA | Fully HIPAA compliant system that operates as a Business Associate of the health system; BAA required prior to go-live |
| Security Compliance | Luma Health is ISO 27001:2022, HITRUST CSF r2, HITRUST ai2, TX-RAMP Level 2 certified and SOC 2 Type II Attested |
| Audit Logging | All agent interactions logged with timestamp, patient identifier, call outcome, and any escalation events |
| Opt-Out | Patients may opt out of AI contact at any time; opt-out is recorded in Epic and honored in all subsequent interactions |
| Data Used for Training | Patient data is never used for model training |
| State Law Compliance | Deploying health systems are responsible for compliance with applicable state telehealth, consent, and AI disclosure laws |

---

## 9. Transparency & AI Disclosure

The agent discloses its AI nature at the start of every interaction.

**English:**
> "Hi, this is an automated assistant calling from [Health System Name] about your upcoming colonoscopy scheduled for [Date]. I'm an AI, not a person — but I'm here to make sure you have everything you need to prepare. You can ask me questions, or I can connect you with a member of our team at any time."

**Spanish:**
> "Hola, soy un asistente automatizado llamando de parte de [Nombre del Sistema de Salud] sobre su colonoscopía programada para el [Fecha]. Soy un sistema automatizado, no una persona, pero estoy aquí para asegurarme de que tenga todo lo que necesita para prepararse. Puede hacerme preguntas o conectarle con un miembro de nuestro equipo en cualquier momento."

Disclosure scripts are customizable by deploying health systems but must retain the AI identification language. Luma recommends that health systems review disclosure requirements under applicable state laws before modifying the default scripts.

---

## 10. Human Oversight & Governance

### 10.1 Recommended Oversight Structure

- A designated clinical or operations lead at the health system should own the agent configuration and review escalation logs weekly during the first 90 days of deployment.
- Staff should receive a minimum of one structured training session on the agent's capabilities, limitations, and escalation protocols prior to go-live.
- Luma Health provides onboarding support and a deployment playbook that includes a recommended governance checklist.

### 10.2 Monitoring & Review Cadence

| Cadence | Activity |
|---------|----------|
| Weekly (Days 1–90) | Review escalation log volume and reason codes; identify script gaps or common patient confusion points |
| Monthly | Review procedure completion and adequate prep rates against pre-deployment baseline |
| Quarterly | Full workflow review; update call scripts based on patient feedback and outcomes data |
| Ongoing | Monitor for Epic chart data quality issues that could cause the agent to receive stale or incomplete prep instructions |

### 10.3 Incident Response

If the agent is found to have communicated incorrect prep instructions or caused patient harm, the health system's existing incident response protocols govern. Luma Health should be notified within 24 hours of any confirmed patient safety event attributed to agent behavior. The open-source nature of the workflow means the broader community can review, flag, and contribute fixes via GitHub.

---

## 11. Open Source Commitments

**What is published on GitHub:**

- Full colonoscopy prep workflow configuration templates for Epic + Luma
- English and Spanish call scripts (all touchpoints)
- Compliance guidelines and regulatory reference notes

Contributions, adaptations, and peer review from health systems, GI societies, and patient advocates are welcomed. All material is published under an open license. Luma Health maintains a core maintainer team responsible for reviewing pull requests and publishing versioned updates.

---

## 12. Limitations Summary

The following limitations are acknowledged by Luma Health and should be understood by all deploying organizations:

1. The agent's accuracy is dependent on the quality and completeness of data in the patient's Epic chart. Incomplete or incorrect prep orders in Epic will result in incorrect patient instructions.
2. The agent supports English and Spanish only. Patients with other primary languages require supplemental human-led outreach.
3. The agent does not have clinical judgment and will not detect whether a patient's reported symptoms warrant urgent evaluation — it will escalate, but cannot triage.
4. The agent cannot verify patient identity beyond what is presented in the Epic scheduling record. Health systems should apply their standard patient identity verification policies.
5. Performance on patients with significant speech or hearing impairments may be reduced. SMS fallback partially mitigates this but is not a complete substitute.
6. The configurable touchpoint schedule means outcome benchmarks will vary across deployments. Published benchmarks reflect specific timing configurations and may not generalize.
7. The system has been designed and tested for colonoscopy prep workflows. Use for other procedure types is not supported in this release and is not recommended without additional validation.

---

## 13. Contact, Versioning & Feedback

| Field | Details |
|-------|---------|
| Maintainer | Luma Health — Workflow Integrations Team |
| Issue Reporting | GitHub Issues (feature requests, script corrections, bug reports) |
| Clinical Questions | Contact your Luma Health implementation team or submit via GitHub Discussions |
| Version History | Maintained in CHANGELOG.md in the GitHub repository |
| Next Planned Review | Q3 2026 — incorporating 90-day deployment outcomes data from initial cohort |

---

## Disclaimer

This system card describes the intended design, capabilities, and limitations of the Luma Health Colonoscopy Prep Coordination Voice Agent. It does not constitute medical advice, legal advice, or a guarantee of clinical outcomes. Deploying health systems are solely responsible for validating that this system is appropriate for their patient population, complying with applicable laws and regulations, and maintaining appropriate clinical oversight. This is a v1.0 document and will be updated as the system evolves.
