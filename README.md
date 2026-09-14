# 🚀 [Clinical Trial Risk Monitor & Protocol Deviation Detector]

> ⚠️ **Replace everything in `[ ]` brackets with your actual content before submission.**

---

## 👥 Team

| Field | Value |
|---|---|
| **Team Name** | [Team Sentinel] |
| **Track** | [AI / DevOps / Sustainability / Open] |
| **Team Lead** | Yug Mandaviya] — [yugmandviya683@gmail.com] |
| **Members** | [Vrund Patel], [Malya Patel], [Shlok Bhatt ] |

---

## 🎯 Problem Statement

> In 2–3 sentences: What problem does your project solve? Who experiences this problem?

[Our project solves the problem of clinical trial protocol deviations — missed visits, wrong dosing, banned co-medications — going undetected until an FDA audit, by continuously checking patient visit records against the protocol and flagging every deviation with a GCP-classified severity and a per-site risk score. This is experienced by clinical trial risk managers and sponsors running large, multi-site trials (5,000+ visits across 200+ sites), who today rely on slow, sampled, retrospective site monitoring instead of real-time visibility. Left undetected, these deviations only surface at audit — where a single rejected submission costs $50–100M and delays approval by 6–12 months.]

---

## 💡 Solution

> In 2–3 sentences: What did you build? How does it solve the problem above?

[We built an end-to-end risk monitoring pipeline: a structured protocol spec defines dosing rules, visit windows, and banned co-medications, a rule-based engine checks every patient visit against that spec and classifies each deviation as Major, Minor, or Administrative per ICH E6 GCP — citing the exact clause violated — and a weighted scoring formula rolls those deviations into an explainable per-site risk score, surfaced live on a sortable risk-ledger dashboard. This solves the problem by replacing periodic, sampled manual review with continuous, auditable checking of every visit at every site: a risk manager can see which sites are highest risk in real time, drill into the specific deviations behind that score, and generate a CAPA-ready report — with root-cause hypothesis and recommended actions — in one click instead of days of manual drafting.]

---

## ✨ Key Features

- **Feature 1:** [Protocol deviation detection: Every patient visit is checked against a structured protocol spec (dosing rules, visit windows, banned co-medications) and flagged the moment it violates a rule — no manual chart review required.]
- **Feature 2:** [GCP severity classification: Each deviation is graded Major, Minor, or Administrative per ICH E6(R2), with the exact clause cited — so every flag is auditable, not a black box.]
- **Feature 3:** [Explainable site risk scoring: A weighted formula (5×Major + 2×Minor + 1×Admin) ÷ visits ranks all 200+ sites by risk, decomposable back into the deviations behind each score.]
- **Feature 4:** [One-click CAPA report generation: For any flagged site, the dashboard drafts a CAPA-ready report — deviation summary, root-cause hypothesis, and recommended actions — in seconds instead of days.]
- **Feature 5:** [Optional]

---

## 🛠️ Tech Stack

| Category | Technologies |
|---|---|
| **Languages** | [HTML, CSS (inline, custom properties)] |
| **Frameworks** | [None (vanilla HTML/CSS static site)] |
| **IBM Technologies** | [IBM Bob (core AI agent, custom skills, protocol analysis mode), watsonx.ai (foundation model inference for deviation classification), Bob Skills API (CAPA generation & protocol parsing), IBM Db2 (CDISC-compliant data store), IBM Cloud (HIPAA-compliant infrastructure)] |
| **Databases** | [] |
| **Other** | [] |

---

## 📁 Repository Structure

```
├── src/                  # All source code
├── docs/                 # Written documentation
│   ├── problem-statement.md
│   ├── solution-overview.md
│   ├── architecture.md
│   └── setup-guide.md
├── demo/                 # Demo artifacts
│   ├── screenshots/      # App screenshots
│   └── demo-video-link.txt  # Link to demo video
├── presentation/         # Slide deck
└── submission.yaml       # Structured submission metadata
```

---

## ⚡ How to Run

> **Copy these exact steps from your [`docs/setup-guide.md`](docs/setup-guide.md)**

```bash
# 1. Clone the repo
git clone https://github.com/[your-repo].git
cd [your-repo]

# 2. Install dependencies
[your install command here]

# 3. Configure environment
cp .env.example .env
# Edit .env with your values

# 4. Run the project
[your run command here]
```

---

## 🖥️ Demo

| Artifact | Link |
|---|---|
| 📹 Demo Video | [See demo/demo-video-link.txt](demo/demo-video-link.txt) |
| 🌐 Live Demo | [See demo/live-demo-url.txt](demo/live-demo-url.txt) |
| 🖼️ Screenshots | [See demo/screenshots/](demo/screenshots/) |
| 📊 Presentation | [See presentation/slides.pdf](presentation/) |

---

## ⚠️ Known Limitations

> Be honest — judges appreciate transparency over overclaiming.

- [Limitation 1:Synthetic data, not a real trial: Patient records are generated (seeded, not live), since real trial data can't be used publicly for privacy reasons — the pipeline is unvalidated against an actual EDC/clinical dataset.]
- [Limitation 2: Recall gap on administrative-only deviations: Detection is 100% precision and 100% recall on safety-relevant (major/minor) deviations, but misses administrative-only issues like missing signatures or late forms (69.5% overall recall) — the current schema has no field to represent document/signature status.]
- [Limitation 3:Only manually spot-checked in a headless browser: Not tested across real browsers/devices beyond the responsive CSS breakpoints written for mobile.]

---

## 🏅 What We're Most Proud Of

[The result that we'd most want judges to look closely at is the validation on Slide 7 / in the Results section: we didn't just build a detector and assume it works — we ran it against ground-truth injected deviations and got 100% precision (zero false positives) and 100% recall on every safety-relevant deviation (wrong dose, banned co-medication, missed safety assessment, visit-window violation). The only misses were administrative-only issues our schema doesn't yet model, and we say so plainly rather than hiding it.

We're equally proud of the design discipline behind that number: every deviation flag traces back to a specific ICH E6(R2) GCP clause, and every site risk score decomposes back into the exact deviations behind it — nothing is a black box an auditor would have to take on faith. For a tool whose whole premise is "don't let the FDA find problems you should have caught yourself," being explainable end-to-end isn't a nice-to-have, it's the actual product.]

---
