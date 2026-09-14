# Solution Overview

## What We Built

[ClinicalGuard AI — a real-time clinical trial risk monitor and protocol deviation detector. It continuously monitors patient visit data across 200+ trial sites, automatically detects when records deviate from the approved protocol, classifies each deviation by regulatory severity, scores site-level risk, and generates FDA audit-ready CAPA (Corrective and Preventive Action) reports — all powered by IBM Bob.]
## How It Works



1.[Data Ingestion — Electronic Data Capture (EDC), CTMS, eCRF, and lab data stream in via HL7 FHIR / API from participating trial sites.]
2.[Normalization — Raw data is standardized into CDISC SDTM/ADaM datasets for consistent downstream analysis.]
3.[Protocol Parsing — Bob reads the trial protocol PDF and extracts its rules as structured, evaluable logic.]
4.[Deviation Detection — Every patient-visit record is evaluated against those rules in real time; any departure is flagged automatically (< 5 seconds).]
5.[GCP Classification — Each flagged deviation is tagged Major / Minor / Administrative per ICH E6 R2 GCP guidelines, with regulatory rationale.]
6.[Site Risk Scoring — A weighted composite score is computed per site using deviation rate, severity mix, enrollment pace, and training status.]
7.[CAPA Report Generation — Bob performs root-cause analysis and produces a structured, audit-ready CAPA PDF with specific corrective and preventive actions — replacing 2–3 days of manual CRA work.]


## Architecture Diagram

> See [`architecture.md`](architecture.md) for the detailed diagram.

[Optionally include a simple ASCII or Mermaid diagram here for quick reference.]

```
[User] → [Frontend: React] → [API: FastAPI] → [watsonx.ai] → [Dashboard]
                                    ↓
                             [PostgreSQL DB]
```

## Key Design Decisions

| Decision | Rationale |
|---|---|
| [Used IBM Bob as the core AI agent] | [Bob's custom skills API enabled rapid scaffolding of domain-specific capabilities (protocol parsing, CAPA generation) without building an LLM pipeline from scratch.] |
| [CDISC SDTM/ADaM as the internal data model] | [These are the FDA-expected submission formats — normalizing to them early means no transformation layer at submission time.] |
| [ICH E6 R2 as the severity taxonomy] | [Using the international GCP standard (rather than a custom rubric) makes outputs directly defensible in regulatory audits.] |

## IBM Technologies Used

[Explain specifically HOW you used each IBM technology — not just that you used it.]

- **[IBM Tech 1, watsonx.ai]:** [: Foundation model inference layer for the ICH E6 GCP deviation classifier — takes a flagged record and outputs the severity tier (Major / Minor / Administrative) with regulatory rationale]
- **[IBM Tech 2, IBM **Bob**]:** [The central AI agent. Used with custom skills for two core tasks: (1) reading a trial protocol PDF and extracting its rules as structured evaluation logic, and (2) generating contextual CAPA recommendations with root-cause analysis. The Bob AI Copilot also answers natural-language queries over trial data (e.g. "Which sites have the most major deviations this month?").]
