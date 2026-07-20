# GitLab Competitive Analysis — Product Marketing Weekly (2021-06-28)

This document distills the competitive-positioning discussion from the GitLab Product Marketing weekly meeting held on June 28, 2021. The conversation centered on the team's competitive-comparison spreadsheet, a new design-team infographic, and the methodology for presenting GitLab against its market rivals.

---

## 1. Competitors Mentioned & Tier Classification

The team has previously tiered competitors into **Tier 1, Tier 2, and Tier 3**. At the time of the meeting, the active work is focused exclusively on **Tier 1**; Tier 2 and Tier 3 are deferred to a later phase.

### Tier 1 Competitors (in scope now)
| Competitor | Bills itself as a "platform"? | Notes |
|---|---|---|
| **GitHub** | ✅ Yes | One of the "platform players"; explicitly called out as selling itself as a platform. |
| **Azure DevOps (ADO / Microsoft)** | ✅ Yes | Platform player; listed on every stage sheet by default. |
| **Atlassian** | ✅ Yes | Platform player, but with a special "coopetition" status (see Section 5). Was almost removed from the list and treated purely as a partner, but was added back because the team decided they cannot be excluded. |
| **JFrog** | ✅ Yes | Platform player; sells itself as an end-to-end platform. |
| **Jenkins** | ❌ No | Does not position itself as a single DevOps platform. |
| **CloudBees** | ❌ No | Closely associated with Jenkins; does not bill itself as a full platform. |

> Samia observed that the spreadsheet had been copy-pasted so that every stage tab showed the same six names (ADO, Atlassian, GitHub, Jenkins, JFrog, CloudBees) — which is incorrect for stages like Monitor or Configure.

### Tier 2 / Tier 3
- Explicitly out of scope for the current iteration. The team said, *"for now we'll move to tier two and tier three then we'll look at those other [competitors]."*
- Stage-specific competitors (e.g., monitoring-native vendors for the Monitor stage, configuration-management vendors for Configure) were mentioned but not named in the meeting; these are expected to enter when the team expands past Tier 1. The team noted that *"we already identified top three competitors for every stage"* in a prior exercise.

---

## 2. Competitive Positioning Approach

The team laid out a deliberate philosophy for how GitLab should be positioned against competitors, which shapes every decision in the spreadsheet and infographic:

- **A helpful industry asset, not a marketing hit piece.** The stated goal is *"a helpful asset for the industry… a helpful page… comparing DevOps tools, not a 'this is all of our competitors and why we're better.'"* This is a direct contrast to the team's instinct to "go hard" with aggressive competitive coloring.
- **Honest, market-lens framing.** Features/capabilities in the matrix must reflect what a buyer is actually shopping for in the market — not GitLab-only checkboxes. The ideal mix includes some features GitLab leads on, some competitors lead on, and some GitLab does not have, so that *"somebody would look at it and be like, this looks honest and trustworthy and like an accurate assessment of the market."*
- **Green-only visual language.** The design team's new infographic intentionally uses **no red** (and for now, no yellow). Fewer green dots = *less good*, not *bad/worse*. The rationale is twofold: (1) it reads as a comparison rather than an attack, and (2) it supports GitLab's many **coopetition** relationships (partners who are also competitors). One PMM noted his personal instinct was to "put some red on there," but deferred to the design recommendation.
- **GitLab as the single application / single source of truth.** Supporting messaging work (discussed in the same meeting) gravitates toward taglines like *"A single source of truth, countless possibilities,"* *"All in one for everyone,"* and *"More speed, less risk"* — all reinforcing the platform/unification differentiator.
- **Call out platform-claim gaps.** For vendors who bill themselves as end-to-end platforms but lack entire stage capabilities (e.g., zero out of 15 monitoring features), the team wants those zeros called out explicitly rather than silently omitted.

---

## 3. Stage-Specific Competitor Mapping

The team organizes the comparison around **GitLab's 10 DevOps stages** (Manage, Plan, Create, Verify, Package, Release, Configure, Monitor, Secure, Protect). Mapping decisions from the discussion:

### Core principle
> *"If the competitor does not apply to that particular stage, they shouldn't be on that tab in the spreadsheet."*

Competitors are **not** forced into every stage. Each stage tab should list only the Tier-1 competitors that actually compete there, and PMMs should leave non-applicable competitors off the tab entirely (no columns to fill in for them).

### Specific stage mappings discussed
- **Monitor (Observability/Incident Management):**
  - GitHub is **not** a relevant Monitor competitor ("we may not have like a GitHub or something like that; we would have monitor-based competitors").
  - CloudBees is likely not relevant for Monitor.
  - "Platform" vendors (GitHub, ADO, Atlassian, JFrog) that claim end-to-end coverage but score **0/15** on Monitor features should have that zero explicitly called out on the Monitor tab.
  - Native monitoring/observability vendors are expected to be added when Tier 2/3 work begins.
- **Configure:**
  - Will also require a different, stage-appropriate set of competitors (not the default six).
  - Zero-score platform vendors (e.g., those with no configure capabilities) should likewise be called out.
- **CD/Release, Verify/CI, SCM/Create, Plan, Secure, Protect, Package, Manage:**
  - Not individually mapped in this segment, but the same rule applies — pull only relevant Tier-1 names onto each tab.

### Future (post-launch) stage presentation
- The infographic launches with a stage-level scorecard.
- **Next iteration:** clicking a stage (e.g., Verify) will drill down to the list of ~15 features, where the team can *"describe what the heck [the stage] is"* in value-based language — addressing the concern that stage names like "Configure," "Monitor," and "Manage" are not self-explanatory to external audiences.

---

## 4. Methodology Decisions

The team made a number of concrete process/modeling decisions for the competitive analysis asset:

1. **Tiered rollout.**
   - Phase 1 (current): **Tier 1 only** — the five/six platform/big-name competitors listed in Section 1.
   - Phase 2+: Add **Tier 2 and Tier 3** competitors (including stage-specific specialists).

2. **Per-stage relevance filtering.**
   - Do **not** copy-paste the full competitor list onto every stage tab.
   - Only include competitors that actually compete in that stage; leave non-applicable ones off entirely.

3. **Stage-based organizing framework.**
   - Use GitLab's existing **10 stages** as the structural backbone. The team considered alternative frameworks but concluded that introducing a new model would add a reconciliation layer and slow progress; instead they are *"trying to move towards some sort of progress."*

4. **~10–15 features per stage, market-defined.**
   - Features should be chosen through a **market/buyer lens** (what a buyer of that capability compares), not a GitLab-product lens.
   - Deliberately avoid making them all "GitLab-only" wins; include areas where competitors lead to build credibility.

5. **Add a GitLab row/column.**
   - GitLab itself must be shown in the matrix; otherwise *"you're basically putting all the things that we're better at"* without a balanced view.

6. **Explicit zero-scores for platform vendors.**
   - When a self-described "platform" has zero capabilities in a stage (0/X features), that gap must be visible — it is a key proof point for GitLab's completeness story.

7. **Separation of data and presentation layers.**
   - The new infographic is built on a data model so the presentation layer (stage labels, colors, layout) can be updated without reworking spaghetti code; stage names/descriptions can evolve later without redoing the underlying comparisons.

8. **Value-based descriptions, not internal jargon.**
   - Stage names alone (Manage, Configure, Monitor) are not descriptive enough. The team agreed to add plain-language, value-based descriptions at the feature-drilldown level so audiences understand what each stage actually does for them.

9. **Visual tone: green-only, no red.**
   - For launch, stick with shades of green to keep the tone educational and non-adversarial; revisit adding red/yellow later once the asset is live.

10. **Top-3 competitors per stage (prior work).**
    - From an earlier (pre-meeting) tiering exercise, the team had already identified top three competitors per stage; this work feeds the current Tier-1 filtering.

---

## 5. Key Competitive Insights (Strategic Notes)

- **Platform-vs.-point-tool wedge is central.** The biggest contrast GitLab can draw is against vendors who *market* themselves as end-to-end platforms (GitHub, Azure DevOps, JFrog, Atlassian) but in reality have zero offerings in whole stages like Monitor and Configure. Calling out "0/X" scores for these vendors turns their own platform positioning against them.

- **Jenkins and CloudBees are different animals.** They do not claim to be full platforms, so the "gap" framing doesn't apply the same way; they compete on specific stages (especially CI/Verify) and should be treated as point/best-of-breed competitors rather than platform rivals.

- **Atlassian is the trickiest coopetition relationship.** The team explicitly debated removing Atlassian from the competitor list entirely and listing them only as a partner (likely due to Jira integration partnerships that are important to GitLab). The conclusion: *"we can't do that"* — Atlassian must be on the competitive list, but the green-only, non-adversarial tone of the asset is specifically designed to keep these partner-competitor relationships workable.

- **GitHub is the primary platform foil.** GitHub is the first vendor named when the team discusses "vendors that bill themselves as a platform" and gaps in Monitor/Configure; it is treated as the lead Tier-1 reference point.

- **Honesty is a competitive weapon in itself.** The team believes an asset that shows GitLab *not* winning on every single feature — one that includes competitor-only strengths — will be more trusted and more widely used than a traditional FUD-style comparison page. This is a deliberate strategic choice to win industry credibility.

- **Naming/messaging reinforces the platform story.** The parallel messaging work in the same meeting converged on *"More speed, less risk"* (speed + security/quality in one), *"A single source of truth, countless possibilities,"* and securing both *"the factory and its deliverables"* — all designed to reinforce the single-application advantage that the competitive matrix is quantifying stage by stage.

- **Stage descriptions are a known weak spot to fix post-launch.** The team acknowledged that stage labels like "Manage" and "Configure" are opaque to buyers; the fix is deferred to v2 (the drill-down feature view) so that the asset can ship on time.

---

*Source: Product Marketing Meeting (weekly) 2021-06-28 — transcript from https://www.youtube.com/watch?v=lBVtvOpU80Q, competitive-discussion segment approximately lines 611–911.*
