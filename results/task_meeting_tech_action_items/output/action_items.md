# Action Items — GitLab Product Marketing Weekly Meeting (2021-06-28)

**Source:** Product Marketing Meeting (weekly) 2021-06-28 — <https://www.youtube.com/watch?v=lBVtvOpU80Q>

## Summary

| Metric | Count |
|---|---|
| **Total action items** | **14** |
| Corporate Events | 3 |
| Product Announcements (GitLab Commit) | 5 |
| Competitive Analysis | 3 |
| New Infographic | 2 |
| Messaging Framework | 1 |

---

## 1. Corporate Events (GTM Event Sponsorship)

The current model is that a GTM team "sponsors" each corporate event, with the PMM and campaign manager providing support. Event assignments so far: Platform → re:Invent, CI/CD → Google Next, GitOps → KubeCon.

### 1.1 All PMMs — Secure campaign manager commitments

- **Owner:** Each PMM (all attendees)
- **Action:** Reach out to your respective campaign managers and get explicit commitment for the event your GTM team is sponsoring; have them comment on the tracking issue confirming "yes, I can commit to this" so the corporate events team has a clear signal of support.
- **Deadline:** Not specified (as soon as possible, to unblock corp events)
- **Context:** Corporate events — locking down event support from GTM teams.

### 1.2 Cindy & William — Pick events if broader feedback doesn't come in

- **Owner:** Cindy and William
- **Action:** If insufficient GTM team members respond, the two of you will directly choose which events to sign up for, with the rest of the team backing you up.
- **Deadline:** Not specified (contingent on response rate)
- **Context:** Corporate events — fallback plan in case GTM team feedback remains thin (only one person had responded at meeting time).

### 1.3 PMMs — Continue cadenced syncs with GTM teams where they still exist

- **Owner:** Each PMM
- **Action:** Use any remaining regular syncs (e.g., the two-week cadence that is still running for some teams) to drive event commitments; note that GitOps syncs were cancelled after enablement, so rely on Slack/async for that track.
- **Deadline:** Ongoing
- **Context:** Corporate events — clarifying which teams still have recurring syncs to coordinate through.

---

## 2. Product Announcements for GitLab Commit

The team is building a "what's shipped in the last year / now fully mature" announcement list for the GitLab Commit event (product keynote + Sid's keynote + PR fodder). The target is ~3 items per stage, themed/bucketed (not tiny MVCs), with excitement levels (1–3) and a final top-5 overall selection for PR.

### 2.1 All PMMs — Complete your stage feature list (3 per stage) with themed buckets and excitement ratings

- **Owner:** All PMMs (per their respective stages)
- **Action:**
  - Populate ~3 announcement items per stage, grouping small MVCs into larger themed "full-fledged feature" stories (e.g., vulnerability management, research-based UX improvements, incident management, GitOps/Kubernetes agent + HashiCorp integrations, pipeline editor, epic boards, etc.).
  - Reuse highlights from the 14.0 launch where applicable and add new items to round out PR coverage across areas.
  - Assign a 1–3 excitement level based on judgment (customer demand/upvotes, MAU where available, customer conversations); treat the rating as a stack rank (one "3" per stage) rather than an absolute score.
- **Deadline:** **Tuesday, June 29, 2021**
- **Context:** Product announcements for GitLab Commit — building the master feature list.

### 2.2 Brian — Add VS Code integrations (community contributions) to the list

- **Owner:** Brian
- **Action:** Add the VS Code integration stories (including the community-contributed extensions that became official in the same month, plus related "juicier" notes) into the Integrations portion of the announcement list.
- **Deadline:** Tuesday, June 29, 2021 (aligned with overall list deadline)
- **Context:** Product announcements — integrations bucket; William flagged VS Code as missing and Brian confirmed he had good notes to add.

### 2.3 Cormac — Add a top-five Plan entry

- **Owner:** Cormac
- **Action:** Add a Plan-stage candidate (e.g., epic boards, milestone burn-up charts, or other long-requested Plan capabilities) into the top-five overall shortlist.
- **Deadline:** Tuesday, June 29, 2021
- **Context:** Product announcements — selecting top-five cross-stage announcements; William explicitly said he would ping Cormac for this.

### 2.4 William — Aggregate the list, re-bucket the last 12 months of shipments, and identify top-five overall

- **Owner:** William
- **Action:** After the team fills in their stages, go through everything shipped in the last 12 months, re-bucket into themes, and narrow the full set (~30 items across 10 boxes) down to a top-five overall list for PR to use at Commit. Reserve a spot for Plan. For security, default to vulnerability management over fuzzing (already had press) for the lead story, with proprietary scanners (Semgrep replacement, DAST/Browser-based SPA scanner) as online items.
- **Deadline:** Pre-Commit (aligned with Tuesday deadline inputs)
- **Context:** Product announcements — consolidation and top-five selection for PR/keynotes.

### 2.5 Team — Quick time-boxed review of top-five candidates

- **Owner:** Entire PMM team
- **Action:** Do a ~5-minute time-boxed pass over the assembled list together (async or in next sync) to align on top-five overall picks (leading candidates discussed: UX improvements, vulnerability management, GitOps/Kubernetes agent + HashiCorp/Terraform integrations, pipeline editor / CI/CD, and a Plan item).
- **Deadline:** Before Tuesday deadline / next sync
- **Context:** Product announcements — aligning on the PR-facing top five.

---

## 3. Competitive Analysis (Competitive Feature Comparison Sheet)

The team is filling out a cross-stage competitive comparison against tier-one competitors (ADO/Atlassian, GitHub, Jenkins, JFrog, CloudBees), covering 10–15 market-oriented features per stage.

### 3.1 Samia (and all PMMs) — Only fill out applicable tier-one competitors per stage

- **Owner:** Samia (and all PMMs filling out their stage tabs)
- **Action:** On your stage's tab, only fill in columns for tier-one competitors that are actually relevant to that stage (e.g., CloudBees isn't relevant for Monitor; different competitors apply to Configure). Leave non-applicable competitors blank rather than forcing a comparison. Tier 2 and tier 3 competitors will be addressed in a later pass.
- **Deadline:** Not specified (current phase of the competitive sheet)
- **Context:** Competitive analysis — Samia raised that every sheet had the same five competitors copy-pasted; William confirmed to only score the relevant ones.

### 3.2 All PMMs — Add a GitLab line/column on each stage tab

- **Owner:** All PMMs
- **Action:** Add a GitLab row/column to each stage's comparison tab so the sheet reads as a balanced market comparison ("honest and trustworthy") rather than just a list of "what we're better at." Include both GitLab-only strengths and areas where competitors lead.
- **Deadline:** Not specified (part of current competitive sheet pass)
- **Context:** Competitive analysis — Samia asked whether to include GitLab; William confirmed yes.

### 3.3 All PMMs — Explicitly call out "zero" platform vendors and use value-based stage descriptions

- **Owner:** All PMMs
- **Action:**
  - For platform vendors (GitHub, Azure DevOps, Atlassian, JFrog) that position themselves as platforms, explicitly call out when they have 0-out-of-15 capabilities in a stage (e.g., monitoring); do not leave this implicit. (Jenkins/CloudBees are excluded because they don't bill themselves as full platforms.)
  - Describe stages using value-based language (e.g., what monitor/configure actually mean to a buyer) rather than just internal stage names ("Manage," "Configure," "Monitor"), so external readers understand what they're looking at.
- **Deadline:** Not specified (current pass)
- **Context:** Competitive analysis — improving credibility/readability of the comparison.

---

## 4. New Infographic (DevOps Tool Comparison)

The design team has produced a new infographic for both single-tool (GitLab-vs-X) comparisons and platform-player comparisons. It uses green shading only (no red/yellow) to read as a helpful industry comparison rather than aggressive competitive shill.

### 4.1 William — Share the infographic link with the team

- **Owner:** William
- **Action:** Post the link to the new design-team infographic (both single-competitor and platform-player versions) in the meeting notes/chat for the team to review.
- **Deadline:** Completed during the meeting (link shared in chat)
- **Context:** New infographic — William found and shared the link live on the call.

### 4.2 PMM / Design team — Plan v2 iteration: click-through feature descriptions for each stage

- **Owner:** PMM team (in partnership with design)
- **Action:** In the next iteration of the infographic (not this launch — v1 needs to ship first), build out the click-through behavior so users can click on a stage (e.g., Verify, Manage, Monitor) and see the list of ~15 features with plain-language explanations of what the stage is and why buyers should care. This addresses the feedback that stage names alone ("Manage," "Configure") are meaningless to external audiences.
- **Deadline:** Post-v1 launch (next iteration, not blocking current release)
- **Context:** New infographic — the team flagged that external audiences won't understand internal GitLab stage names; William confirmed the data/presentation layer separation makes this update easier than in the old spaghetti-code version.

---

## 5. Messaging Framework

William shared a draft messaging framework with three pithy pillars and asked the team for quick input. The consensus pick for the velocity/security pillar was "More speed, less risk."

### 5.1 William — Finalize and submit the messaging framework

- **Owner:** William
- **Action:** Finalize the three-pillar messaging framework (consensus direction: (1) "A single source of truth, countless possibilities," (2) "End-to-end control over your software factory," (3) **"More speed, less risk"** for the velocity/security/quality pillar) and submit it. Drop weaker variants ("scale up, speed up, test up"; "stay on track"; overlong phrasings) in favor of the shorter, parallel noun-phrase style.
- **Deadline:** **End of day, Monday June 28, 2021**
- **Context:** Messaging framework — assignment handed to William late the prior week; due EOD same day as the meeting.

---

*Compiled from meeting transcript. Deadline dates inferred from meeting date (Monday, June 28, 2021): "Tuesday" = June 29, 2021; "end of day" = June 28, 2021.*
