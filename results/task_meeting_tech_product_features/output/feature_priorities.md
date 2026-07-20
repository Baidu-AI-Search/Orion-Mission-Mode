# GitLab Commit Conference — Feature Priorities

**Source:** Product Marketing weekly meeting, June 28, 2021 (GitLab Commit prep)
**Purpose:** Prioritized list of product features/improvements to highlight at the upcoming GitLab Commit conference — feeding PR, the product keynote, and Sid's keynote.

---

## Evaluation Methodology (Team Agreement)

The group agreed on the following approach for selecting and evaluating features:

1. **Look back ~12 months (since ~13.0 / last year), not just 14.0.**
   Because GitLab ships tiny MVCs on a public roadmap, individual releases rarely contain "GA-announceable" features. The goal is to identify capabilities that *started* as MVCs over the past year but have now matured into full-fledged, production-ready features — the equivalent of what another company would announce as "GA at the event."
2. **Bundle MVCs into themes/capabilities.**
   Rather than highlighting individual small MVCs, roll up several related iterations into a single named capability (e.g., "Vulnerability Management," "Research-based UX," "Incident Management," "GitOps — K8s Agent + HashiCorp integrations"). Each bucket gets one umbrella name with "key iterations" listed underneath. This mirrors how other companies (e.g., Windows 11) launch — a few big themes, each backed by a laundry list of improvements.
3. **Target ~3 items per stage** (Create, Secure, Monitor, Plan, CI/CD, Configure/GitOps, Manage, Integrations, Verify/CD, etc.) so PR has fodder to hit different press beats.
4. **Excitement level 1–3 (judgment-based, stack-ranked).**
   - **3 = most exciting** — headline-worthy, customers are "hyper-excited," high upvotes/demand, strong MAU, or something that legitimately feels new and cool.
   - **2 = solid/worthwhile** — useful but not a rooftop shout-out (e.g., fixing something broken, incremental but important).
   - **1 = filler** — included only when a stage has "slim pickings" so PR can still see the full spectrum.
   - Scoring is a judgment call informed by: customer demand/upvotes, MAU/adoption, anecdotal customer excitement, and whether it's genuinely new vs. fixing something broken. If a stage produces enough candidates, each stage gets *one* 1, one 2, one 3 (i.e., treated as a **stack rank**, not an absolute scale — one PMM's "3" is not automatically more important than another's).
5. **Reuse 14.0 highlights** where applicable, then add a few more to cover additional press areas and stages (Manage, Integrations were specifically called out as missing from the initial draft).
6. **Final step: stack-rank across stages into a Top 5 overall** for the keynotes (product keynote + Sid's keynote) — the "announcements of the event" that press will care about.
7. **Output format:** The list will eventually move from a doc to a spreadsheet so the team (with PR) can stack-rank ~30 items down to the final keynote picks.

---

## Prioritized Feature List

### 1. Research-based / End-to-End UX Improvements (Create + cross-stage)
- **Stage/area:** Create (flagship), with UX improvements spanning multiple stages
- **Excitement level:** 3
- **Description:** A broad bundle of UI/UX polish and redesign work delivered across the year, rolled up as a single "new user experience" story. The presenter initially worried this was just "fixing something that's broken" (and thus a 2), but concluded several pieces are genuinely cool and that big platform launches (Windows 11 was cited) routinely lead with UX as a flagship pillar.
- **Notes:** Maps to more than just the Create stage, which made it an easy Top-5 pick — it can anchor a cross-product story. William/SCM owner presented this as his model example of bucketing key iterations under one umbrella name.

### 2. Vulnerability Management
- **Stage/area:** Secure
- **Excitement level:** 3
- **Excitement rationale:** Cindy's lead example of why bundling MVCs matters — many small MVCs shipped all year, but viewed end-to-end the product went "from this to this," which is far more compelling than any single MVC.
- **Description:** Matured end-to-end vulnerability management capability, assembled from a year of small iterative releases (many of which landed in the 14.0 launch).
- **Notes:**
  - Chosen over fuzzing and over the newer proprietary scanners as Secure's #1 pick.
  - Group agreed fuzzing is "worn out" — the fuzzing acquisitions (last spring/summer ~2020) already got acquisition press and a follow-up integration press cycle, so a third bite at the apple is unlikely.
  - Noted as a carry-over/reuse from the 14.0 launch highlights.

### 3. GitOps Bundle: Kubernetes Agent + HashiCorp/Terraform Integrations
- **Stage/area:** Configure / GitOps
- **Excitement level:** 3
- **Description:** A combined GitOps story consisting of (a) the **Kubernetes Agent** (Ks Agent / GitLab K8s Agent), in which the team has invested heavily and which has strong community traction, and (b) the **HashiCorp/Terraform integration** — a previously community Terraform module that became officially supported, which is the "yes/no line" for enterprise adoption.
- **Notes:**
  - William explicitly bucketed these as "GitOps — K8s Agent + HashiCorp integrations" rather than separate items.
  - Terraform official-support angle parallels the VS Code story (community → official).
  - Cormac noted many customers are already using the Terraform integration.
  - Samia noted the GitOps team's regular sync was cancelled after enablement; KubeCon is GitOps' sponsored event.

### 4. Pipeline Editor
- **Stage/area:** CI/CD (Verify)
- **Excitement level:** Not explicitly scored in the transcript, but put forward as CI/CD's headline candidate for the Top 5.
- **Description:** New editor for authoring CI/CD pipelines, positioned as a flagship CI/CD improvement.
- **Notes:** Explicitly named by William as the CI/CD option for the Top 5 ("pipeline editor is definitely an option for ci cd").

### 5. Plan / Planning-stage Flagship (placeholder — to be finalized by Cormac)
- **Stage/area:** Plan
- **Excitement level:** Reserved for a Top-5 spot.
- **Description:** The team explicitly agreed "we should reserve a spot for Plan" and that Cormac would add a top Plan pick. Likely candidates discussed under Plan:
  - **Epic Boards** — users have been asking for them for years; high demand/upvotes (a strong candidate for excitement 3 on demand alone, per the upvotes heuristic).
  - **Milestone Burn-up Charts** — important monthly feature but judged a 2 ("helpful but not very exciting"); better bundled with other Plan items to "up-level" it.
  - Cormac was going to aggregate 12 months of Plan shipments and re-bucket after the meeting.
- **Notes:** Value Stream Analytics was also floated as a possible Plan/Manage story, but customizable Value Stream Analytics shipped in 12.9 (outside the ideal window), which was noted as a bummer.

### 6. Incident Management
- **Stage/area:** Monitor
- **Excitement level:** Treated as Monitor's lead bucket (pattern matches Vulnerability Management — multiple MVCs rolled up).
- **Description:** A bundle of incident-management features released across many months; the core existed in 12.x but was fleshed out into a full story over the past year.
- **Notes:** Samia confirmed she used the same bucketing approach for Monitor as William did for Create/SCM.

### 7. Proprietary DAST Scanner ("Browser" / Berserker)
- **Stage/area:** Secure (DAST)
- **Excitement level:** 3 (Connie's personal #2 pick for Secure, behind Vulnerability Management)
- **Description:** GitLab's own proprietary DAST scanner (codenamed "Browser" — heard on the audio as "berserker"), purpose-built to scan single-page applications (SPAs), which represent a unique scanning challenge. In beta at the time of the meeting.
- **Notes:** Paired in discussion with the Semgrep-based SAST scanner replacement as "proprietary scanning" advances.

### 8. Semgrep-based SAST Scanner Replacement
- **Stage/area:** Secure (SAST)
- **Excitement level:** Not explicitly rated; positioned as an online/press-note item rather than a keynote headliner.
- **Description:** Replaced one of GitLab's existing SAST scanners with Semgrep, part of the broader move toward proprietary scanning IP.
- **Notes:** Seen as something that "could get picked up as an online item" alongside a couple of the GitLab 14 pieces, but not the lead Secure story.

### 9. VS Code Integrations (community-contributed, now official)
- **Stage/area:** Create / Integrations (Ecosystem)
- **Excitement level:** Not explicitly rated; flagged by Brian as "juicier" than just "we have a VS Code extension."
- **Description:** Two VS Code integration improvements that shipped in the same month: (a) a long-standing community VS Code extension becoming officially supported, and (b) additional deeper integration work.
- **Notes:**
  - Brian: "One other cool thing about that is both of those vs code integrations that hit the same month were community contributions."
  - Parallels the Terraform story: community-built → officially supported → enterprise-usable.
  - Brian had not yet added this to the list at meeting time but said he would add it with good notes.

### 10. Jira Integration Improvements
- **Stage/area:** Integrations (Ecosystem)
- **Excitement level:** Not explicitly rated.
- **Description:** Improved Jira integration, called out alongside VS Code as one of the standout integrations worth highlighting.
- **Notes:** William flagged that Integrations had been underrepresented on the initial draft but that "we have some pretty exciting integrations — the Jira stuff is pretty good."

### 11. Manage Stage Items
- **Stage/area:** Manage
- **Excitement level:** TBD
- **Description:** Placeholder — the team noted Manage was missing from the initial draft and needed to be added. William specifically said "we need to add Manage so i see manage in here that's excellent," implying a PMM had just added it but specifics were not discussed on the call.
- **Notes:** Value Stream Analytics (customizable) was discussed in the Plan/Manage vicinity but was flagged as having shipped in 12.9, making it slightly stale for a "past year, now mature" Commit narrative.

### 12. UX / Getaway Cluster Key Iterations (SCM/Create sub-items)
- **Stage/area:** Create (SCM)
- **Excitement level:** Bundled under the UX headline (item #1).
- **Description:** William called out "several key iterations aligned with getaway cluster" and "several that roll up to a research-based user experience" as his example of how to group SCM features.
- **Notes:** Shared on the call as the model approach for how all stages should bucket their MVCs.

---

## Top 5 Overall Picks for the Commit Keynote

The team live-narrowed the list to an intended Top 5 for the product keynote (and Sid's keynote). Based on the discussion, the five slots are:

1. **Research-based / Cross-stage UX Improvements** (Create-led, cross-stage) — easy pick because it spans stages, not just Create, and is a classic launch pillar.
2. **Vulnerability Management** (Secure) — beat out fuzzing and the proprietary scanners as Secure's strongest story; bundles a year of MVCs into a clear "from-this-to-this" narrative.
3. **GitOps: Kubernetes Agent + HashiCorp/Terraform Integrations** (Configure/GitOps) — strong enterprise customer usage (Terraform) and heavy investment + community momentum (K8s Agent).
4. **Pipeline Editor** (CI/CD / Verify) — the clear CI/CD candidate called out on the call.
5. **Plan-stage flagship** (Plan) — **reserved but not finalized** at meeting time; Cormac to aggregate and fill this slot (Epic Boards a leading candidate given long-standing demand; Milestone Burn-up Charts likely bundled in as a supporting iteration). William explicitly said: "we should reserve a spot for plan."

Supporting/secondary press items (not keynote headliners, but fodder for online coverage): Semgrep-based SAST scanner, additional GitLab 14 odds-and-ends, VS Code integrations.

---

## Explicitly Deprioritized Features & Reasons

| Feature / Item | Stage | Reason for Deprioritization |
|---|---|---|
| **Fuzzing / Fuzzing acquisitions integration** | Secure | Already exhausted two press cycles — the original acquisition (last spring/summer ~2020) and a follow-up integration piece the PR team asked for. Connie: "it's kind of worn out now… we already did press on so we probably will not get another chomp at that." |
| **Individual MVCs in isolation** | All stages | Team agreed the public roadmap + tiny-MVC shipping cadence means individual features ship in a "not really usable" state; highlighting them one-by-one doesn't land. Must be bundled into mature capability stories. |
| **Milestone Burn-up Charts (as a standalone headline)** | Plan | Cormac: "that's a two… helpful… but it's not very exciting." Better bundled with other Plan items to raise the story's level rather than featured alone. |
| **"We fixed something broken" UX items (treated as default 2s, not 3s)** | Cross-stage | Initial instinct was to score many UX improvements as a 2 ("we're almost fixing something that's broken, that's not something you want to shout from the rooftops"). Overridden in aggregate (the bundled UX story was promoted to a 3), but individual fix-it items are not to be standalone highlights. |
| **Log segments** | (implied ops/monitoring adjacent) | William: "we're not going to highlight log segments" — included only to show the full spectrum when a stage has slim pickings, not as a real candidate. |
| **Excitement-1 items in general** | All stages | If a stage has enough material, 1s exist only to pad the spectrum for stack-ranking; they are not real keynote/PR candidates. If a stage has "double-digit entries," stack rank and drop the 1s entirely. |
| **Customizable Value Stream Analytics** (as a fresh Commit story) | Plan/Manage | Shipped in 12.9 — outside the ideal "past year, now mature" window, which made it awkward as a flagship; William: "it's a bummer that customizable value stream analytics is 12.9." Still noted as a possible story if aggregated with other analytics work. |

---

## Open Action Items Captured During the Discussion

- **Cormac:** Aggregate Plan stage items from the last 12 months, re-bucket, and fill the reserved Top-5 Plan slot (likely Epic Boards as lead, with Milestone Burn-up Charts bundled underneath).
- **Brian:** Add VS Code integrations (including the community-contribution angle) to the list; also review Integrations items (Jira, etc.).
- **All PMMs:** Fill in ~3 items per stage (one 1, one 2, one 3) using the bucketing methodology; confirm GTM/campaign-manager sign-off for sponsored events (Platform:Reinvent, CI/CD:Google Next, GitOps:KubeCon).
- **William:** Move the list to a spreadsheet so the team + PR can stack-rank ~30 items down to the final keynote Top 5; due Tuesday after the meeting.
