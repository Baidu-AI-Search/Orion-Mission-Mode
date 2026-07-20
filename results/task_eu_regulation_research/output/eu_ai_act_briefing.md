# EU AI Act Compliance Briefing
## Regulation (EU) 2024/1689 — Guidance for an AI-Powered Developer Tools SaaS

**Audience:** Legal, Product, Engineering, and GTM leadership at a SaaS company selling AI-powered developer tools (e.g., an AI coding assistant) to European customers via API.
**Date:** 16 July 2026
**Status:** Working briefing — reflects the Regulation as adopted (OJ L 2024/1689) and the May/June 2026 political agreement on the **AI Omnibus (Omnibus VII)** that reschedules several application dates.

---

## Executive Summary

The EU AI Act is the world's first comprehensive horizontal AI law. It applies extraterritorially to any provider placing AI systems or general-purpose AI (GPAI) models on the EU market, and to any deployer using them in the EU, regardless of where the company is incorporated (Art. 2). For a SaaS company offering an AI coding assistant over API, three buckets of obligations are most relevant today:

1. **Transparency obligations (Art. 50)** — users must be informed they are interacting with AI; synthetic/AI-generated content must be labelled. Application accelerated by the Omnibus to **2 December 2026**.
2. **GPAI "downstream" duties (Arts. 53–55)** — when the company does not train its own foundation model but consumes one via API, it acts as a *deployer of a GPAI model* (and possibly a *provider of an AI system*). Documentary and informational duties flow down the value chain from the upstream model provider.
3. **AI literacy (Art. 4)** — the company and its customers must ensure staff have a sufficient level of AI understanding; applicable from **2 August 2026** for most obligations already in force.

Critically, a general-purpose **AI coding assistant is not, by default, a high-risk AI system**. High-risk status attaches only if the tool is placed on the market or used for one of the use cases listed in **Annex III** (biometrics, critical infrastructure, education/vocational training, employment, essential services, law enforcement, migration/border, justice/democracy) or as a safety component of a regulated product under **Annex I**. Coding assistants used to generate code for general software development fall, in most deployments, into the **limited-risk (transparency) category** — provided that appropriate disclosures and safeguards are in place.

The maximum administrative fine is **€35 million or 7% of worldwide annual turnover** for prohibited-practice violations, **€15 million or 3%** for most other non-compliance, and **€7.5 million or 1.5%** for supplying incorrect/misleading information (Art. 99).

---

## 1. Risk Classification (Arts. 5, 6, Annexes I & III)

The Act uses a **four-tier risk pyramid** tied to the degree of threat to health, safety, and fundamental rights.

| Tier | Examples | Regulatory stance | Key article |
|---|---|---|---|
| **Unacceptable risk** | Manipulative/subliminal techniques causing significant harm; exploitation of vulnerable groups (age, disability, socio-economic status); social scoring by public authorities; predictive policing based solely on profiling; untargeted scraping of facial images; emotion recognition in the workplace/education (except medical/safety); biometric categorisation inferring sensitive traits; real-time remote biometric identification in public spaces for law enforcement (narrow exceptions) | **Banned outright** | Art. 5 |
| **High risk** | (a) AI systems that are safety components of products covered by EU harmonised product legislation in **Annex I** (toys, machinery, medical devices, motor vehicles, aviation, lifts, etc.), requiring third-party conformity assessment; (b) standalone AI in eight listed domains in **Annex III**: biometrics, critical infrastructure, education/vocational training, employment/HR, access to essential private/public services, law enforcement, migration/asylum/border control, administration of justice & democratic processes | Allowed only if a full set of ex-ante and ex-post requirements is met; pre-market conformity assessment; EU database registration | Arts. 6–27, Annex III |
| **Limited risk** | AI systems that interact with natural persons (chatbots, virtual assistants, coding assistants), systems generating synthetic content (text, image, audio, video), emotion recognition / biometric categorisation systems used outside the prohibited contexts, and AI used to generate or manipulate "deepfake" content | **Transparency obligations only** (inform users; label synthetic content) | Art. 50 |
| **Minimal / no risk** | Spam filters, AI-enabled video games, most productivity tools that pose no material risk to rights/safety | **No mandatory obligations** beyond existing law; voluntary codes of conduct encouraged | Recitals 127–129, Art. 69 |

### Where does an AI coding assistant fall?

For a typical SaaS coding assistant (autocompletion, code explanation, refactoring, unit-test generation, agentic coding workflows) sold through API for general developer use:

- **Default classification: limited-risk AI system** under Art. 50(1), because it (a) interacts directly with natural persons (developers), and (b) generates synthetic content (code snippets, explanatory text). It is **not** listed in Annex III.
- **Not a safety component** of an Annex I product unless integrated as a safety-critical element of, e.g., a medical device, aircraft, or vehicle — which is the customer's, not the vendor's, use case.
- **High-risk trigger at customer level:** if a customer uses the API *inside* a high-risk use case — e.g. an enterprise integrates the assistant into an automated CV-screening pipeline (Annex III, employment), or a bank uses it to make consumer-credit decisions (Annex III, essential services) — the **customer becomes a deployer** (or provider, per Art. 25 "change of own purpose" rules) and inherits high-risk duties. You do not automatically become a high-risk provider merely because a customer repurposes the tool, but your terms of service, documentation, and downstream information pack (Art. 53) must make those boundaries clear.
- **Possible prohibited-practice exposure:** the product must avoid any of the Art. 5 red lines — for example, it must not perform emotion recognition on developers using the tool in an employment setting, nor scrape biometric databases without consent to build code-style profiling.

---

## 2. Obligations by Risk Tier

### 2.1 Prohibited AI practices (Art. 5) — applicable since 2 Feb 2025

These practices **cannot** be placed on the market, put into service, or used in the EU at all. Relevant for a coding-tools vendor:

- **Art. 5(1)(a)–(b):** Do not use subliminal, manipulative or deceptive techniques, or exploit vulnerabilities (e.g. juniors' inexperience, cognitive overload) in ways likely to cause material harm.
- **Art. 5(1)(f):** Emotion recognition in the workplace / in education is prohibited — do not instrument the IDE or code review flow to infer developer mood, stress, or engagement without explicit medical/safety justification.
- **Art. 5(1)(g):** Do not use biometric categorisation to infer race, religion, political opinions, union membership, sexual orientation, or sex life from users' voices or faces.
- Note the Omnibus (May 2026) agreement **adds new prohibitions**: AI generating CSAM (child sexual abuse material) and non-consensual intimate imagery; expected application **2 Dec 2026**.

### 2.2 High-risk AI systems (Arts. 8–27) — staged application

Even though a coding assistant is not normally high-risk, awareness is essential because enterprise customers (in HR, finance, healthcare, critical infrastructure) may attempt to put the tool to high-risk use. Providers of high-risk systems must comply with the **"seven pillars"**:

1. **Risk management system** maintained throughout the lifecycle (Art. 9).
2. **Data and data governance**: training, validation and testing datasets must be relevant, sufficiently representative, error-free, and complete with respect to intended use; biases against protected groups must be assessed (Art. 10).
3. **Technical documentation** drawn up before placing on the market and kept up to date (Art. 11, Annex IV).
4. **Automatic logging / record-keeping** to ensure traceability (Art. 12).
5. **Transparency and provision of information to deployers**: clear instructions for use, including intended purpose, performance characteristics, known limitations, human-oversight requirements, expected lifetime, and foreseeable misuse (Art. 13).
6. **Human oversight** designed so that natural persons can understand, supervise, and override outputs (Art. 14).
7. **Accuracy, robustness and cybersecurity** at appropriate levels, plus resilience against attacks, data poisoning, evasion attacks, and model-drift monitoring (Art. 15).

High-risk providers must also run a **conformity assessment** (Art. 43) — either internal control (Annex VI) for most Annex III systems or third-party notified-body assessment for safety components and certain biometric uses — affix the **CE mark**, register the system in the **EU AI database** (Art. 49), and maintain post-market monitoring (Art. 72) and a serious-incident reporting channel (Art. 73).

**Deployers** of high-risk systems have their own independent duties under Art. 26: use per instructions; assign trained human overseers; keep logs for at least 6 months; monitor for incidents and report them; inform workers/employee representatives before workplace use; and (for public bodies and essential-services operators) perform a **Fundamental Rights Impact Assessment (FRIA)** under Art. 27.

### 2.3 Limited-risk AI systems — **your main bucket today** (Art. 50)

- **Art. 50(1):** Where an AI system interacts directly with natural persons, the provider must **inform** those persons that they are interacting with AI — unless it is obvious from the circumstances. The disclosure must be clear, timely, and intelligible.
- **Art. 50(2):** Providers of AI systems generating **synthetic content** (text, images, audio, video) must ensure outputs are labelled in a machine-readable format and detectable as artificially generated or manipulated — i.e., **watermarking / content provenance metadata**. The Commission's draft guidelines (June 2026 consultation, Art. 50(4)) elaborate metadata standards (likely aligned with C2PA).
- **Art. 50(3):** Deepfakes (content that "realistically" appears to depict persons doing/saying things they did not) require **explicit, prominent disclosure** at the point of publication, subject to carve-outs for legitimate artistic/satire/legally authorised law-enforcement uses.
- **Art. 50(4):** The Commission (via the AI Office) will issue (and under Omnibus, may continue refining) guidelines, codes of practice and harmonised standards for watermarking.

### 2.4 Minimal-risk AI systems

No mandatory AI-Act-specific obligations. The Act encourages voluntary adherence to codes of conduct (Art. 69). Note that other EU laws (GDPR, CRA, NIS2, DSA, product safety, consumer protection) continue to apply.

---

## 3. Timeline — Key Compliance Deadlines

The Act entered into force on **1 August 2024** (20 days after publication in the OJ on 12 July 2024). It applies in stages. The May/June 2026 **AI Omnibus (Omnibus VII)** political agreement, finalised by Council on 29 June 2026, rephases certain dates for high-risk systems and for Art. 50(2) labelling. The table below reflects that sequencing (still pending formal publication in the OJ — companies should monitor the final text):

| Date | What applies | Article |
|---|---|---|
| **2 Feb 2025** | **Prohibited AI practices (Art. 5) + AI literacy (Art. 4)** | Arts. 4, 5; Art. 113(1)(a) |
| **2 Aug 2025** | **GPAI / foundation-model rules (Chapter V)**, governance bodies (European AI Office, Member State authorities, AI Board), penalties framework (Chapter XII, except Art. 101 for GPAI fines, which aligns with GPAI application), confidentiality (Art. 78), notified-body/notifying-authority provisions | Arts. 51–56; Art. 113(1)(b) |
| **2 Aug 2026** | **General application date** of the Act — most obligations take effect (definitions, governance, many non-Annex-III rules, AI-literacy enforcement). Member States must have at least one AI regulatory sandbox running by this date. | Art. 113(1) main text |
| **2 Dec 2026** (revised by Omnibus) | **Art. 50 transparency obligations fully apply** — including machine-readable labelling / watermarking of AI-generated content and new prohibitions on AI-generated CSAM / non-consensual intimate content | Revised Art. 113 per Omnibus |
| **2 Dec 2027** (revised by Omnibus; previously 2 Aug 2026 / 2 Aug 2027) | **Stand-alone Annex III high-risk AI systems** (biometrics, critical infrastructure, education, employment, essential services, law enforcement, migration, justice) — full obligations apply | Art. 6(1) as amended by Omnibus |
| **2 Aug 2028** (revised by Omnibus; previously 2 Aug 2027) | **High-risk AI systems that are safety components of products regulated under EU sectoral legislation** (Annex I — e.g., medical devices, machinery, cars, aviation) | Art. 6(1) / Annex I as amended; transitional carve-out for machinery (sector-specific requirements increasingly handled through the Machinery Regulation under Omnibus "sector exit") |
| 31 Dec 2030 | Legacy: certain AI systems that were components of large-scale IT systems already in use before 2 Aug 2027 must be brought into compliance | Art. 83 transitional |

> **Operational takeaway:** The near-term cliff for a coding-assistant SaaS is **2 December 2026** (transparency/watermarking) — not the high-risk dates. The company should treat 2 August 2026 as the internal "Act is live" milestone for AI literacy and governance processes.

---

## 4. General-Purpose AI (GPAI) and Foundation Models (Chapter V, Arts. 51–56)

### 4.1 Definitions

- **General-purpose AI (GPAI) model** (Art. 3(1)(dc)): an AI model — including when trained with large-scale self-supervision on broad data — that exhibits **significant generality**, i.e., it can competently perform a wide range of distinct tasks regardless of how the data was presented, and can be integrated into downstream systems.
- A **foundation model** is a large-scale GPAI model; the Act treats these under the same GPAI framework, with heightened duties for those that pose **systemic risk** (Art. 51). A model is presumed to pose systemic risk when its training compute exceeds **10²⁵ FLOPs** (Commission may update the threshold), or when the AI Office so designates based on capabilities/impact.

### 4.2 Obligations on GPAI model *providers* (Art. 53) — already in force since 2 Aug 2025

1. Maintain, document, and make available up-to-date **technical documentation** sufficient for downstream providers/deployers to understand the model's capabilities, limitations, and risk-mitigation measures, and to comply with their own obligations.
2. Provide **information to downstream providers and deployers** in a clear, accessible form, including: capabilities, limitations (known hallucination rates, domain cut-offs, energy/compute profile), foreseeable risks, recommended uses, alignment/evaluation results, and content-moderation/safety controls.
3. Comply with **EU copyright law**, in particular the DSM Directive (Directive (EU) 2019/790): the provider must have a publicly available policy to comply with copyright and to honour **rights-reservation / opt-outs** expressed by rightsholders in machine-readable form (e.g., robots.txt, well-known protocols) — see Art. 53(1)(d) and the Code of Practice (GPAI Code of Practice, finalised in 2025).
4. Provide a **sufficiently detailed summary of training content** (the "training data summary") publicly.
5. For systemic-risk GPAI models (Art. 55): perform **model evaluations**, assess and mitigate systemic risks (e.g., large-scale disinformation, cybersecurity enablement, critical-infrastructure harm, biological/chemical misuse), conduct **adversarial testing**, report serious incidents, ensure an adequate level of **cybersecurity**, and provide reporting to the AI Office.
6. **Open-source carve-out** (Art. 53(2)): GPAI models released under a **permissive open licence** with publicly available weights and sufficient information to reproduce them are **exempt from the Art. 53(1) documentation/downstream-information duties** (except copyright) — *unless* the model poses systemic risk, in which case the Art. 55 duties still apply.

### 4.3 Implications for an API-based coding-assistant vendor

Your company is most likely **not a GPAI model provider** if it consumes a third-party foundation model (e.g., through an LLM vendor API) and layers prompt engineering, retrieval, agent orchestration, and IDE tooling on top. However:

- You are a **provider of an AI system** (the coding assistant) and must still satisfy Art. 50 transparency and any other system-level obligations.
- You are a **downstream deployer/integrator** of the upstream GPAI model. You must:
  - **Obtain and retain** the upstream provider's Art. 53 documentation pack and downstream information. Large model vendors already publish model cards/system cards and updated API terms to meet these duties; the SaaS company should extract those into its own customer-facing materials.
  - **Contractually require** the model vendor (via DPA/MSA amendments or API terms addenda) to pass through the information required by Art. 53 and to notify you of incidents/safety updates; ensure right to audit / evidence of copyright-compliance policy.
  - **Not substantially modify** the model in ways that would recharacterise you as a GPAI provider under Art. 25/Art. 51 (e.g., further training the foundation model on huge corpora at >10²⁵ FLOPs generally does not happen in a typical SaaS fine-tune, but full continued pre-training could).
  - **Fine-tuning/routing/rag** above the API: these usually create a derivative *AI system*, not a new GPAI model, but should be documented so that the company has an auditable view of what is "yours" vs. the upstream model.
- If you **distribute** the coding assistant to enterprise customers who themselves may embed it in high-risk workflows, your Art. 13-style instructions (for any high-risk downstream adaptations) and your ToS must set out permitted uses, prohibited uses, and known limitations, so that your enterprise customers can discharge their Art. 26/27 duties.

The **Code of Practice on General-Purpose AI Models** (finalised under Art. 56 by the AI Office) provides a safe-harbour-style implementation pathway: adhering to it may be relied on to demonstrate compliance with Chapter V until harmonised standards are adopted.

---

## 5. Transparency Requirements (Art. 50) — What You Must Disclose, When

### 5.1 Disclosures when users interact with AI (Art. 50(1))

- **When:** At the latest when the interaction begins (e.g., first chat/response in the IDE assistant, first autocomplete suggestion tooltip).
- **What:** A clear, conspicuous statement that the user is interacting with an AI system — for example, a label on the assistant panel such as "AI assistant (beta): suggestions may be incorrect or incomplete; always review."
- **Exception:** No disclosure is required when the AI nature is obvious from the context and a reasonable person would already know. A coding assistant is not "obviously" AI if presented as if generated by a human co-worker — an explicit label is safer.
- **Who bears the duty:** The *provider* (you). Deployers are not separately required to re-notify end-users beyond what is already in the interface, but customers may add their own notices.

### 5.2 Labelling AI-generated content (Art. 50(2))

Applicable from **2 Dec 2026** (revised by Omnibus):

- Outputs (code, commit messages, docstrings, summaries, natural-language explanations, pull-request descriptions) must be **marked as AI-generated** in a **machine-readable format** and be detectable.
- Recommended approach: embed provenance metadata following emerging standards (C2PA, IPTC, open web metadata); for code, consider comments in generated snippets (`// Generated by [Product] — review before use`) plus digital-content credentials.
- The duty sits with the **provider** that generates/serves the content; downstream users (e.g., a developer who edits the code before committing) are not expected to re-label every line, but if they substantially edit and republish, they must not remove or falsify provenance marks.

### 5.3 Deepfake disclosures (Art. 50(3))

Where the tool generates or manipulates realistic image/audio/video depicting real persons (less common for coding assistants, but relevant if you add voice/avatar features), **explicit, prominent disclosure** must be made at point of publication. Exceptions apply for law enforcement, art/satire, and certain journalistic contexts, with carve-outs for freedom of expression.

### 5.4 Transparency guidelines

The Commission issued draft Art. 50 implementation guidelines for consultation in **June 2026**; final guidelines are expected before 2 Dec 2026. They will cover labelling granularity, metadata schemas, and when the "obvious" exception applies. The company should monitor the final version and align UI labels and metadata accordingly.

### 5.5 Additional transparency-related duties

- **High-risk deployers** in the workplace must inform workers/employee representatives (Art. 26(5)(f)) — relevant to enterprise customers rolling out coding assistants to their engineering teams (you should make a template notice available).
- **Emotion recognition / biometric categorisation** outside the prohibited contexts still requires disclosure (Art. 50(1)(b)–(c)).

---

## 6. Penalties (Art. 99)

The Act sets three tiers of administrative fines, in each case **whichever is higher** between the fixed amount and the percentage of global annual turnover:

| Violation | Fine cap | SME/start-up mitigation |
|---|---|---|
| **Prohibited practices (Art. 5)** or certain GPAI systemic-risk duties under Art. 55 | **Up to €35,000,000** or **7% of total worldwide annual turnover** | Caps may be proportionally lower for SMEs/startups, but authorities retain discretion (Art. 99(6)–(8)) |
| **Non-compliance with other obligations** (e.g., high-risk requirements in Arts. 8–27, GPAI obligations under Art. 53, transparency duties under Art. 50, AI literacy under Art. 4) | **Up to €15,000,000** or **3% of total worldwide annual turnover** | Same SME proportionality |
| **Supplying incorrect, incomplete, or misleading information** to notified bodies, national competent authorities, or the AI Office | **Up to €7,500,000** or **1.5% of total worldwide annual turnover** | Same |

Additional enforcement levers:

- **Proportional, dissuasive, effective**: Member States set national enforcement rules and penalties (Art. 99(9)).
- **Interim measures & market withdrawal**: Authorities can order withdrawal from the EU market, corrective actions, or temporary bans.
- **Aggravating factor**: Repeat or intentional infringements attract higher penalties; voluntary disclosure and remedial action can be mitigating.
- **Personal data angle**: Where non-compliance also violates GDPR, regulators (DPAs and AI authorities) will coordinate; cumulative exposure is possible.

---

## 7. Practical Steps — What the Company Should Do Now

The following plan is sequenced against the statutory/Omnibus deadlines and scoped realistically for a non-high-risk SaaS that consumes third-party GPAI via API.

### Phase 1 — Immediate (before 2 Aug 2026)

1. **Classify the product formally.**
   - Document, in a memo signed off by Legal + Product, that the coding assistant is a **limited-risk AI system** under Art. 50 (not high-risk under Annex III, not a prohibited Art. 5 practice). Re-run this classification for each major feature (agents, voice, PR-review bots, etc.) and for each vertical use-case you sell into.
   - Maintain a **use-case register** of customer-reported deployments; flag any that touch Annex III domains and escalate to Legal.

2. **AI literacy (Art. 4)** — build a light-touch programme now (this has been effective since 2 Feb 2025).
   - Train engineering, product, sales, support, and security staff on what the model can/cannot do, hallucination risks, copyright risks, and the basics of the AI Act.
   - Track completion; keep records.
   - Provide a customer-facing one-page "AI literacy primer" so end-user organisations can discharge their own Art. 4 obligations.

3. **Transparency UI changes (Art. 50(1))** — ship before the Aug 2026 general date.
   - Add a persistent "AI" label and short explanatory text to the assistant pane.
   - Disclose limitations ("Suggestions may be incorrect, insecure, or infringe third-party rights; always review and test.") at the point of first use and in docs.
   - Add an opt-out / feedback mechanism for users who wish to report harmful output.

### Phase 2 — Before 2 Dec 2026 (transparency/watermarking D-day)

4. **Implement machine-readable content labelling (Art. 50(2)).**
   - Add provenance metadata (C2PA-style "AI-generated" marker) to any exported/generated artefacts: code snippets (start/end marker comments), generated PR descriptions, chat exports, generated documentation, image/audio outputs.
   - Build detection/verification endpoint so customers and platforms can read the markers.
   - Monitor the Commission's final Art. 50 guidelines and the harmonised standards (CEN/CENELEC) for conformance.

5. **Deepfake policy (Art. 50(3)).** If any roadmap item creates realistic synthetic media, ship a prominent disclosure layer and a policy on prohibited use.

6. **Terms of service & acceptable-use policy** updates:
   - Prohibit Art. 5 use cases (social scoring, workplace emotion recognition, CSAM, NCII, biometric scraping).
   - Disclaim high-risk use of the general-purpose API without additional contractual safeguards; offer an enterprise "high-risk addendum" for customers that need to use the tool in Annex III contexts (with technical and contractual controls).
   - Indemnify/IP terms: address copyright claims from AI-generated code; align with your upstream provider's indemnity.

### Phase 3 — GPAI supply chain and downstream information (already applicable since 2 Aug 2025; ongoing)

7. **Map your GPAI supply chain.**
   - Identify every model vendor, version, and data source you rely on.
   - From each vendor, obtain the Art. 53 documentation pack (model card, training-data summary, copyright policy, evaluation results, known risks and mitigations, incident reporting channel).
   - Amend contracts/DPAs to require: upstream compliance with the Act, pass-through of safety updates, prompt incident notification (including systemic-risk incidents under Art. 55(4)), and cooperation in regulatory inquiries.

8. **Build a "downstream information pack" for your own customers**, analogous to what you receive from model vendors, covering:
   - System description, intended purpose, foreseeable misuse, performance and limitations (hallucination, security-vulnerability rate if measured, supported languages/frameworks).
   - Human-oversight instructions (review, test, don't commit un-reviewed code; enable CI gating; restrict production database access from agentic actions).
   - Technical contact for serious-incident reporting (mirroring Art. 73).
   - Copyright and training-data disclosures appropriate to your fine-tuning/RAG data.

### Phase 4 — High-risk-readiness (trigger-based, before any customer signs an Annex III use case)

9. **Pre-build a high-risk "shell"** so the company can react quickly if a regulated customer (bank, hospital, recruiter, public body) asks for Annex III-grade evidence:
   - Draft Annex IV technical documentation template (risk management per Art. 9, data governance Art. 10, logging Art. 12, transparency Art. 13, human oversight Art. 14, accuracy/robustness/cybersecurity Art. 15).
   - Implement logging sufficient to maintain audit trails (inputs, outputs, model version, policy actions) — useful even outside high-risk contexts for debugging and safety.
   - Establish a post-market monitoring system (Art. 72) and an incident response / serious-incident notification workflow (Art. 73) — these are also best practice for general SaaS reliability.
   - Prepare a Fundamental Rights Impact Assessment (FRIA, Art. 27) template for public-sector deployers.
   - Decide on a conformity-assessment strategy: self-assessment under Annex VI for most Annex III systems, or engage a notified body if/when you ship a safety component.

10. **Cybersecurity & model safety (Art. 15, Art. 55).**
    - Harden the API against prompt injection, data exfiltration, supply-chain attacks on model weights, and jailbreaks.
    - Conduct red-team / adversarial testing at least annually; track and patch regressions when vendor models are updated.
    - For agentic coding features (long-horizon tool use, filesystem/CI access), implement explicit human approval gates (Art. 14-style "meaningful human control") and sandboxing.

### Phase 5 — Governance and ongoing compliance

11. **Appoint an EU compliance lead / Authorised Representative.** If the company has no EU establishment, you must designate an **authorised representative in the Union** (Art. 24) before placing AI systems on the EU market. The representative should be a contact point for Member State authorities and the AI Office.

12. **Register with national authorities / EU AI Office as appropriate.** Registration in the EU database is mandatory for high-risk systems; for limited-risk systems it is not, but maintain a voluntary file for traceability.

13. **Engage with an AI regulatory sandbox.** Member States must operate sandboxes by 2 Aug 2026 (Art. 57). Sandboxes give real-time guidance, derogations for testing, and a safe channel for iterative product development — particularly useful for agentic features.

14. **Adopt recognised standards and codes.**
    - Align your management system with **ISO/IEC 42001** (AI management system), **ISO/IEC 23894** (AI risk), **ISO/IEC 27001/27701** (infosec/privacy), and **NIST AI RMF**.
    - Follow the **GPAI Code of Practice** (Art. 56) as a safe harbour for supply-chain/copyright duties.
    - Consider certifying under the **AI Act voluntary codes of conduct** for non-high-risk systems (Art. 69).

15. **Monitor legislative churn.**
    - Final adoption and entry into force of the **AI Omnibus** (expected summer 2026 in OJ).
    - The upcoming **AI Liability Directive (AILD)** and updates to the **Product Liability Directive** — these add fault-based and strict-liability regimes for AI harm; design your logging and incident-response with civil liability in mind.
    - Harmonised standards (CEN/CENELEC) — once published in the OJ, conformity gives a presumption of compliance.

---

## Appendix A — Quick Reference: Articles Most Relevant to This Company

| Topic | Article / Annex |
|---|---|
| Scope / extraterritorial reach | Art. 2 |
| Definitions (AI system, provider, deployer, GPAI, etc.) | Art. 3 |
| AI literacy | Art. 4 |
| Prohibited practices | Art. 5 |
| High-risk classification | Art. 6, Annexes I & III |
| High-risk provider obligations: risk management / data / documentation / logging / transparency / human oversight / accuracy | Arts. 9–15 |
| Deployer obligations | Art. 26 |
| Fundamental rights impact assessment | Art. 27 |
| Transparency (chatbot disclosure, AI-content labelling, deepfakes) | Art. 50 |
| GPAI model rules (general) | Arts. 51–53 |
| GPAI with systemic risk | Art. 55 |
| GPAI Code of Practice | Art. 56 |
| Regulatory sandboxes | Art. 57 |
| EU database registration | Art. 49 |
| Post-market monitoring & incident reporting | Arts. 72–73 |
| Authorised representative (non-EU providers) | Art. 24 |
| Penalties | Art. 99 |
| Entry into force / phased application | Art. 113 |

## Appendix B — Sources and Further Reading

1. Regulation (EU) 2024/1689 of the European Parliament and of the Council (Artificial Intelligence Act), OJ L, 12 July 2024.
2. European Commission, European AI Office — https://digital-strategy.ec.europa.eu/en/policies/ai-office
3. European Commission, *Draft Guidelines on the implementation of the transparency obligations for certain AI systems under Article 50 of Regulation (EU) 2024/1689* (consultation draft, June 2026).
4. European AI Office, *Code of Practice on General-Purpose AI Models* (finalised 2025, Art. 56).
5. Council of the EU, press release on the AI Omnibus (Omnibus VII) provisional agreement (7 May 2026) and final Council approval (29 June 2026).
6. European Commission, *Guidance on Prohibited AI Practices under Article 5* (Feb 2025).
7. Standardisation requests to CEN/CENELEC/ETSI for harmonised standards under the AI Act.
8. ISO/IEC 42001:2023 — Artificial intelligence management system.
9. AFME, *Response to Commission Consultation on AI Transparency Guidelines under Article 50* (3 June 2026).
10. FERMA, *The EU's Artificial Intelligence Act — Policy Note* (Oct 2024).

> **Disclaimer:** This briefing summarises the EU AI Act and the 2026 Omnibus political agreement for internal planning purposes. It is not legal advice. The company should obtain formal legal opinions from EU counsel before making conformity or product-launch decisions, particularly as the Omnibus text is finalised and as delegated acts, guidelines, and harmonised standards continue to evolve throughout 2026–2028.
