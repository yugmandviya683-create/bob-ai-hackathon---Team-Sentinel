# Architecture

## System Architecture



graph TD
    A[EDC / CTMS / eCRF / Lab Systems] -->|HL7 FHIR API| B[Data Ingestion Layer]
    B --> C[CDISC Normalization\nSDTM / ADaM]
    C --> D[IBM Bob Agent]
    P[Protocol PDF] -->|Bob reads & parses| D
    D --> E[Deviation Engine\nRule Evaluation per Patient-Visit]
    E --> F[GCP Classifier\nwatsonx.ai — Major / Minor / Admin]
    F --> G[Site Risk Scorer\nWeighted Composite Score]
    G --> H[CAPA Report Generator\nBob Skills API]
    G --> I[Real-Time Dashboard\nSponsor / CRA View]
    H --> J[Audit-Ready PDF / Word Export]
    D -->|Natural language queries| I

## Components

| Component | Technology | Responsibility |
|---|---|---|
| Frontend | [e.g., React 18] | [e.g., Dashboard UI, user interaction] |
| Backend API | [e.g., FastAPI] | [e.g., Business logic, orchestration] |
| AI / ML | [e.g., watsonx.ai] | [e.g., Anomaly scoring, classification] |
| Database | [e.g., PostgreSQL] | [e.g., Storing pipeline events and scores] |
| Notifications | [e.g., Slack API] | [e.g., Alerting on threshold breaches] |

## Data Flow

1.Ingestion — EDC, CTMS, eCRF, and lab systems push patient-visit records to the platform via HL7 FHIR REST endpoints.
2.Normalization — Raw records are transformed into CDISC SDTM (raw observations) and ADaM (analysis-ready) datasets, stored in IBM Db2.
3.Protocol Parsing — On trial setup, IBM Bob reads the protocol PDF and extracts eligibility criteria, visit windows, dosing rules, prohibited medications, and ICF requirements as structured, evaluable logic.
4.Deviation Detection — For every new or updated patient-visit record, the Deviation Engine evaluates it against the parsed protocol rules; any mismatch raises a deviation event (< 5 sec latency).
5.GCP Classification — Each deviation event is passed to watsonx.ai, which classifies it as Major / Minor / Administrative per ICH E6 R2 with a regulatory rationale string.
6.Risk Scoring — The Risk Scorer aggregates all open deviations for a site and recomputes its composite score, updating the dashboard in near real time.
7.CAPA Generation — For each deviation (especially Major), Bob performs pattern analysis across the site's history and produces a structured CAPA recommendation: root cause, corrective action, preventive action, and responsible party.
8.Export — The CAPA report is exported as a structured PDF/Word document with full audit trail, timestamps, and version history — ready for FDA submission.
## Security Considerations

All IBM Cloud services accessed via IAM service credentials stored in environment variables — never hardcoded or committed to source control.
IBM Db2 connections use TLS 1.2+ in transit; data at rest is encrypted on IBM Cloud.
21 CFR Part 11 controls enforce timestamped audit trails on every data write, supporting non-repudiation requirements for FDA-regulated environments.
Role-based access control (RBAC) gates dashboard views and CAPA write access by role: Clinical Research Associate (CRA), Sponsor, and Principal Investigator (PI) see different data scopes.
HIPAA-compliant IBM Cloud infrastructure ensures Protected Health Information (PHI) never leaves a compliant boundary.
Protocol PDFs and patient records are processed in-memory and not persisted outside IBM Db2's compliance boundary.
## Scalability Notes

The architecture is designed to scale incrementally across three phases:

Phase 1 (Hackathon MVP): Single-tenant, static demo data, Bob agent running locally via Bob Skills API.
Phase 2 (Beta): Live HL7 FHIR connectors to real EDC/CTMS systems; deviation processing becomes event-driven (stream per site). Bob agent scales horizontally — each trial gets its own agent context.
Phase 3 (Production): Multi-sponsor SaaS on IBM Cloud with isolated Db2 schemas per sponsor. watsonx.ai inference calls are the primary throughput bottleneck and would benefit from request batching and async queuing. The Risk Scorer and CAPA Generator are stateless Bob skills and scale horizontally without coordination. Global support targets 1,000+ sites across multiple concurrent trials.


