# GitLab Product Marketing Weekly Meeting — Decisions
**Meeting date:** June 28, 2021
**Source:** [Product Marketing Meeting (weekly) 2021-06-28](https://www.youtube.com/watch?v=lBVtvOpU80Q)

---

## Summary

A total of **23 decisions/consensus items** were identified across the five agenda topics (corporate events sponsorship, GitLab Commit product announcements, competitive analysis methodology, infographic design, and the messaging framework exercise).

**Items that need further confirmation or follow-up** (tentative or with open action items):
- **#4** Fallback plan for William & Cindy to self-select events if GTM teams don't respond — depends on response rate.
- **#10** The "Top 5" GitLab Commit announcements list — Cormac still needs to add the Plan-stage entry; final stack-ranking still being aggregated.
- **#20** Value-based stage descriptions for the competitive analysis/infographic — deferred to the next iteration (not in the current release).
- **#24** Revisiting the infographic color palette — explicitly a "see how it goes" decision, open to revisiting post-launch.
- **#26** Pillar 2 (control/security) messaging phrasing — strong alignment on Cindy's "secure the factory and its deliverables" concept but wording ("deliverables") still unresolved.

**Action items with deadlines:**
- Commit product announcement inputs due **Tuesday** (July 6, 2021 week — exact date not specified).
- Messaging framework submission due **end of day (June 28, 2021)**.
- PMMs to get campaign manager commitments confirmed on the corporate events issue.

---

## 1. Corporate Events Sponsorship

### Decision 1 — GTM-led event sponsorship model
- **Decision:** The Go-To-Market (GTM) team signs up for and "sponsors" each corporate event; the supporting PMM and their campaign manager run the campaigns and support for that event.
- **Context:** After restructuring, event support processes were not nailed down; the team needed a clear ownership model.
- **Participants involved:** William (facilitator), Cindy, wider PMM team.
- **Status:** Final.

### Decision 2 — Event-to-stage assignments confirmed
- **Decision:** The three corporate events are assigned as follows:
  - **Platform** → AWS re:Invent
  - **CI/CD** → Google Cloud Next
  - **GitOps** → KubeCon
- **Context:** Slack thread had been farming events out; Tai had already populated owners in the issue.
- **Participants involved:** William, Tai (populated assignees), Cindy, PMM team.
- **Status:** Final.

### Decision 3 — Campaign managers must explicitly commit on the issue
- **Decision:** Each PMM will get their campaign manager to explicitly comment on the event issue confirming they can commit to supporting the event (so the corporate events team has a clear, trackable commitment).
- **Context:** Corporate events team does a lot of "cat herding" and needs firmer commitments than Slack-only outreach; only async (Slack/issue) feedback had been gathered so far.
- **Participants involved:** William (requested), all PMMs with event assignments.
- **Status:** Final (action item).

### Decision 4 — Fallback selection if GTM teams don't respond
- **Decision:** If sufficient GTM feedback is not received, Cindy and William will pick the events themselves and GTM teams can back them up.
- **Context:** Only one GTM response had been received at time of meeting; regular GTM syncs are on a two-week cadence (GitOps sync cancelled after enablement).
- **Participants involved:** William, Cindy.
- **Status:** Tentative (contingent on lack of responses).

---

## 2. Product Announcements for GitLab Commit

### Decision 5 — Announcement time frame mirrors GitLab 14 launch
- **Decision:** Commit announcements will look at capabilities that shipped as small MVCs over roughly the past year (since ~13.0) but have **now reached maturity / full-fledged feature status** (what another company would announce as "GA coming out of beta").
- **Context:** GitLab's public roadmap and MVC model make it hard to do traditional "big bang" product announcements at events; the team needed a narrative angle for Commit that would earn press attention.
- **Participants involved:** William, Cindy.
- **Status:** Final.

### Decision 6 — Bundle MVCs into themed narratives; reuse 14.0 content
- **Decision:** Rather than highlighting individual tiny MVCs, PMMs will group related iterations under higher-level themes (e.g., "Vulnerability Management"). Items from the 14.0 launch will be reused and augmented with additional items to give the PR team breadth across coverage areas.
- **Context:** Highlighting single MVCs felt underwhelming; vulnerability management was cited as a feature that landed across many releases but tells a strong year-over-year story.
- **Participants involved:** William, Cindy, Cormac, Brian.
- **Status:** Final.

### Decision 7 — Target ~3 items per stage; add Manage and integrations
- **Decision:** Each stage should aim for ~3 announcement items. The Manage stage must be added. Integrations (VS Code, Jira, Terraform — especially community-contributed → officially-supported stories) should be included as notable items.
- **Context:** Initial draft was missing Manage; integrations (notably two VS Code contributions that shipped the same month, plus the Terraform module moving from community to officially supported) have strong narrative value (community → enterprise-ready).
- **Participants involved:** William, Cindy, Brian, Cormac.
- **Status:** Final.

### Decision 8 — Excitement rating is judgment-based and treated as a stack rank
- **Decision:** The 1–3 "excitement level" is a judgment call informed by MAU (where available), upvotes/community demand, and direct customer feedback — not a strict formula. It will be treated as a **stack rank** (PMMs are constrained on how many 1s/2s/3s they can give out) rather than an absolute score, to prevent inflation.
- **Context:** Samia asked how excitement should be measured; Harsh had suggested stack-ranking; Brian had given a UX bundle a 3 despite initially feeling some items were "fixing what's broken."
- **Participants involved:** William, Samia, Brian, Harsh (via comment), Cormac.
- **Status:** Final.

### Decision 9 — Deliver a Top 5 overall for PR and keynotes
- **Decision:** From the ~30 stage-level items, the team will surface a **Top 5 overall** list to feed PR and the product keynote (and ideally Sid's keynote) — the "announcements of the event."
- **Context:** PR can only effectively promote a small set of stories; the goal is to give Christie & the product keynote a focused set of mature "now ready" stories.
- **Participants involved:** William, Cindy, Brian, Cormac, wider PMM team.
- **Status:** Final.

### Decision 10 — Tentative Top 5 composition
- **Decision:** Working Top 5 discussed in the meeting:
  1. UX improvements (cross-stage bucket — mirrors how other vendors launch, e.g., Windows 11)
  2. Vulnerability Management (security)
  3. GitOps bundle: Kubernetes Agent + HashiCorp/Terraform integrations
  4. CI/CD Pipeline Editor
  5. A Plan-stage item (to be added by Cormac)
- **Context:** Real-time brainstorm; Cormac flagged Plan items like epic boards (long-requested) and milestone burn-up charts; Value Stream Analytics was mentioned as a possible add-on story.
- **Participants involved:** William, Cindy, Cormac, Brian.
- **Status:** **Tentative / needs follow-up** — Cormac still needs to identify and add the Plan entry; William will aggregate across the full shipped-in-last-12-months list before finalizing.

### Decision 11 — Deprioritize fuzzing; Semgrep & browser-based DAST as secondary
- **Decision:** The fuzzing acquisition story will not be in the Top 5 (already covered by prior press and "worn out"). Semgrep (replacement SAST scanner) and the new proprietary browser-based DAST scanner (for single-page apps, currently in beta) will be positioned as secondary/online items rather than top-billed announcements.
- **Context:** Cindy noted fuzzing press was already done; Brian offered Semgrep and browser-based DAST ("Berserker") as strong but perhaps secondary stories; Vulnerability Management was chosen over proprietary scanners for the top security slot.
- **Participants involved:** William, Cindy, Brian.
- **Status:** Final.

### Decision 12 — Commit announcements due Tuesday
- **Decision:** Inputs for the Commit announcement list are due Tuesday (following the meeting).
- **Context:** Timeline to feed PR and keynote planning.
- **Participants involved:** William, all PMMs.
- **Status:** Final (deadline).

---

## 3. Competitive Analysis Methodology

### Decision 13 — Each stage tab only includes relevant Tier-1 competitors
- **Decision:** On the competitive spreadsheet, each stage's tab should only include Tier-1 competitors that actually compete in that stage. Competitors from the master list that don't apply should be left blank/skipped (not forced into comparison).
- **Context:** Samia noticed the same five competitors (ADO/Atlassian, GitHub, Jenkins, JFrog, CloudBees) had been copy-pasted onto every tab — including Monitor, where many are irrelevant.
- **Participants involved:** Elita/Alita (spreadsheet owner), Samia, William.
- **Status:** Final.

### Decision 14 — Add GitLab as a line item on each tab
- **Decision:** Add a GitLab row on each stage tab so the comparison shows GitLab's own capabilities alongside competitors (avoids a one-sided "here's what we're better at" view).
- **Context:** Samia asked whether GitLab itself should be listed; Brian and William affirmed.
- **Participants involved:** Samia, Brian, William, Elita/Alita.
- **Status:** Final.

### Decision 15 — Market-lens framing; include both GitLab-only and competitor-only items
- **Decision:** Features in the comparison should be framed through a market/buyer lens (what a buyer shopping for this solution would care about). It is acceptable — and desired — for some rows to show GitLab-only capabilities and others to show competitor-only capabilities, to look honest, trustworthy, and accurate.
- **Context:** Without this guardrail, PMMs would only list items GitLab is strong on, reducing credibility.
- **Participants involved:** William, Brian, Samia.
- **Status:** Final.

### Decision 16 — Explicitly call out zero scores for platform vendors
- **Decision:** For vendors that position themselves as platforms (GitHub, Azure DevOps, Atlassian, JFrog — i.e., all Tier-1 except Jenkins and CloudBees), PMMs should explicitly call out where they score 0/15 (e.g., no monitoring capabilities), rather than omitting or downplaying.
- **Context:** These vendors claim platform breadth; highlighting gaps in areas like Monitor or Configure is a competitive point that should be visible.
- **Participants involved:** William, Samia, Brian.
- **Status:** Final.

### Decision 17 — Phased rollout: Tier-1 first, then Tier-2 and Tier-3
- **Decision:** Current pass focuses **only on Tier-1** competitors. Tier-2 and Tier-3 competitors will be added in later phases.
- **Context:** Samia asked whether the full competitor list (tiered previously) was in scope; the team is focused on Tier-1 to ship progress.
- **Participants involved:** Elita/Alita, Samia, William.
- **Status:** Final (phased approach).

### Decision 18 — Atlassian retained as a Tier-1 competitor
- **Decision:** Atlassian stays on the Tier-1 list and must be included on the relevant tabs (not treated as a partner-only).
- **Context:** William had wanted to remove Atlassian and treat them purely as a partner but concluded during prior discussions that this wasn't feasible; he confirmed he had already added them back to the sheet.
- **Participants involved:** William, Brian.
- **Status:** Final.

### Decision 19 — Move from stage-name labels to value-based descriptions
- **Decision:** Stage labels (e.g., "Configure," "Monitor," "Manage") should ultimately be described in value-based language that explains what the stage means to buyers, rather than relying on internal GitLab stage names.
- **Context:** Brian pointed out that "Configure" and "Monitor" — and especially "Manage" — are not self-explanatory to external audiences and weaken the asset's impact.
- **Participants involved:** Brian, William.
- **Status:** **Needs follow-up** — deferred to the next iteration (see Decision 22); not blocking the current release.

---

## 4. Infographic Design Feedback

### Decision 20 — Organize infographic around GitLab's 10 stages; use data-model architecture
- **Decision:** The competitive infographic will be organized around GitLab's existing 10 stages (rather than inventing a new taxonomy). It is built on a data model that separates the presentation layer from the data layer, so labels/categories can be updated later without "spaghetti code" changes.
- **Context:** The design team shared a new draft; the team debated whether stage names (Configure, Monitor, Manage) are externally understandable and whether a different organizing framework would be better.
- **Participants involved:** William, Brian, Cindy, design team.
- **Status:** Final (for this iteration).

### Decision 21 — Drill-down feature detail deferred to next iteration
- **Decision:** The click-through to the list of 15 features (which provides the "what is this stage / why do I care" explanation) will **not** ship in the current version; the team will get the current page live first and add the drill-down in the next iteration.
- **Context:** Brian raised the audience-understanding concern; the design lead (William relayed) had already flagged this but said it couldn't be built into the current iteration.
- **Participants involved:** William, Brian, design team.
- **Status:** Final (explicit scope cut for v1).

### Decision 22 — Ship with green-only color palette (no red/yellow) for v1
- **Decision:** The infographic will ship with the design team's recommended green-only palette (no red cells calling out competitors as "bad"). This frames the asset as a helpful, trustworthy industry comparison rather than a "marketing shill" hit piece, which also supports GitLab's coopetition relationships.
- **Context:** The designers chose no-red; William's personal instinct was to go harder with red/yellow; the group agreed the green approach reads as "less good" rather than "terrible," which better fits a comparison-vs-competitive tone.
- **Participants involved:** William, Brian, Cindy, design team.
- **Status:** Final for v1; **tentative for future** — explicitly "see how it goes," open to revisiting after launch.

---

## 5. Messaging Framework Exercise

### Decision 23 — Pillar 1 approved: "A single source of truth. Countless possibilities."
- **Decision:** The first messaging pillar is locked as **"A single source of truth. Countless possibilities."**
- **Context:** William proposed several pithy lines for three framework boxes; this noun-phrase pairing was widely liked as catchy and pithy.
- **Participants involved:** William, Cindy, Brian, Cormac.
- **Status:** Final.

### Decision 24 — Pillar 2 direction: secure/control the factory AND what it produces
- **Decision:** Pillar 2 (control/security) will center on Cindy's turn of phrase from her 14.0 blog post — the idea of **securing/controlling both the software factory and what it produces** (i.e., not just the pipeline, but the deliverables/products coming out of it). William's original "End-to-end control over your software factory" was seen as missing the security/deliverables half.
- **Context:** Brian proposed combining into "secure and control your software factory and its deliverables"; several participants disliked the word "deliverables" (sounds like car manufacturing / corporate jargon) and brainstormed alternatives ("software," "product," "IP") without landing on a replacement.
- **Participants involved:** William, Cindy, Brian, Cormac.
- **Status:** **Tentative / needs follow-up** — concept is agreed, exact wording is still being refined.

### Decision 25 — Pillar 3 approved: "More speed, less risk."
- **Decision:** The velocity/security pillar is locked as **"More speed, less risk."**
  - Rejected alternatives: "Move fast with confidence" (lacks parallel structure with the other noun-phrase pillars); "Velocity with confidence" / "Increase speed and stay on track" (clunky); "More speed, less risk, with confidence" / "High confidence" (overused by competitors); "No trade-offs" (engineers know there are always trade-offs — violates GitLab's "mitigate risk / less risk" communication norm).
- **Context:** The team wanted parallel grammatical tone across the three pillars and language that wasn't already generic in the devtools market. The race-car/bicycle analogy (speed without crashing due to built-in testing & security) anchored the concept.
- **Participants involved:** William, Cindy, Brian, Cormac (Parker mentioned offhand).
- **Status:** Final.

### Decision 26 — Messaging submission due end of day
- **Decision:** William will finalize and submit the messaging framework (untagging himself from follow-up) by end of day June 28, incorporating the chosen lines.
- **Context:** Assignment came in late Thursday/Friday; deadline was end of day Monday.
- **Participants involved:** William.
- **Status:** Final (deadline met same day).

---

## Appendix — Participants referenced in the meeting

- **William** — meeting facilitator (PMM lead)
- **Cindy** — senior PMM
- **Brian** — PMM
- **Cormac** — PMM (Plan stage)
- **Samia / Samya** — PMM (CD, Configure, Monitor)
- **Elita / Alita** — owner of the competitive analysis spreadsheet
- **Tai** — populated event assignees
- **Harsh** — commented on stack-ranking excitement scores
- **Parker** — referenced briefly
- **Christie & Noob** — product keynote at Commit (referenced, not present)
- **Sid** — CEO (keynote, referenced)
- **Ash Withers** — messaging reference (not present)
- **Design team** — infographic owners
