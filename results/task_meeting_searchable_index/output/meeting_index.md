# Meeting Index — GitLab Product Marketing Weekly (2021-06-28)

> A searchable reference index for the meeting transcript found at <https://www.youtube.com/watch?v=lBVtvOpU80Q>. Use this document to quickly locate who said what, which topics were discussed, what decisions were made, and what actions are outstanding.

---

## Metadata

- **Meeting Title:** Product Marketing Meeting (weekly)
- **Date:** 2021-06-28
- **Source:** <https://www.youtube.com/watch?v=lBVtvOpU80Q>
- **Participants (named in transcript):**
  - William (host / facilitator, opens the call and leads discussion)
  - Cynthia ("Cindy")
  - Tai
  - Samya
  - Brian
  - Cormac
  - Harsh
  - Elita
  - Christie (referenced re: product keynote at Commit)
  - Sid Sijbrandij (referenced re: keynote at Commit)
  - Ash Withers (referenced for messaging/turn-of-phrase style)
  - Parker (mentioned in passing during "no trade-offs" banter)
- **Unnamed / referenced parties:**
  - Corporate events team (the "cat herders" running event logistics)
  - Go-to-market (GTM) teams / campaign managers
  - PR team
  - Design team (built the new competitive infographic)
  - "Newb" (likely "Nubu" — discussed with Christie for the Commit product keynote; name unclear from transcript)

---

## Topic Index

1. **Opening / agenda-setting** — William notes the team does not have a ton of items, mentions he might try one "fun" item if there is time, and begins walking through topics.
2. **Corporate events sponsorship & GTM team ownership** — The team discusses how event support has not been fully nailed down post-restructuring; the current tactic is that each Go-To-Market (GTM) team sponsors an event (signs up, with their PMM and campaign manager supporting), and assignments are being tracked in a Slack thread + issue.
3. **Event-to-team assignments** — Current assignments surfaced: Platform → re:Invent, CI/CD → Google Next, GitOps → KubeCon. PMMs are asked to get explicit commitments (e.g., a comment on the issue) from their campaign managers so the corporate events team can plan.
4. **Sync cadences** — Teams are on roughly a two-week sync cadence; the GitOps sync was cancelled after enablement.
5. **Product announcements for Commit — framing** — Brian added a topic on product announcements. The discussion frames this as similar to the GitLab 14 launch: look back over the past ~year (since 13.0) at features that shipped as small MVCs and have since matured / are exiting beta, so they are "announceable" at the Commit event (where GitLab will have a stage and press attention).
6. **Grouping MVCs into themes ("key iterations")** — Cindy proposes grouping small MVCs into themes (e.g., vulnerability management) rather than highlighting individual tiny releases; Brian shares the SCM approach of rolling multiple key iterations up under umbrella labels (e.g., a Getaway/GitOps cluster bucket, a research-based UX bucket).
7. **Reusing 14.0 material & gaps (Manage, Integrations)** — Team agrees 14.0 launch items can be reused and augmented; Manage stage was missing and should be added; Integrations (Jira, VS Code) are called out as potentially exciting. The VS Code integrations are noted as community contributions that became official, similar to the Terraform module going from community-supported to officially supported.
8. **Excitement levels (1–3) and ranking** — Excitement level is acknowledged as partly a judgment call, informed by MAU (where available), upvotes/demand, and customer anecdotes; Harsh suggested stack-ranking. The team pivots from a raw 1–3 score to a constrained stack rank, and ultimately to identifying a "top five overall" list that feeds PR and keynote selection.
9. **Top-five overall announcement shortlist (Commit)** — Candidates discussed for top-five: UX improvements (cross-stage, easy pick), a security item (vulnerability management chosen over fuzzing — already press-worn — and over proprietary scanners like Semgrep/DAST "Browser"/"Berserker"), a GitOps bucket (Kubernetes Agent + HashiCorp/Terraform integrations), a CI/CD item (Pipeline Editor a candidate; Value Stream Analytics mentioned with regret that customizable VSA landed in 12.9), and a Plan item (Epic Boards a long-awaited candidate — Cormac is pinged to add a Plan pick).
10. **"Assembly" company event** — William recommends watching Assembly, describes it fondly as a look-back at wins (with grade-school-carpet-square nostalgia), and notes it reinforces the value of celebrating wins as a company. (Details not shareable on YouTube/public.)
11. **Competitive comparison spreadsheet — stage-relevant competitors only** — Samya asks Elita whether every stage sheet should list the same five competitors or only the relevant Tier 1 competitors. Clarification: only Tier 1 competitors applicable to that stage need to be filled out; later they will move to Tier 2/3. Sheets were initially copy-pasted with the full set (ADO, Atlassian, GitHub, Jenkins, JFrog, CloudBees) — PMMs should strip out the irrelevant ones per stage.
12. **Competitive sheet — GitLab row & honesty** — A GitLab row should be included. Features should be viewed through a market lens (not all GitLab-only); a mix of GitLab-leading, GitLab-gapped, and competitor-leading items makes the comparison honest and trustworthy. Platform vendors (GitHub, Azure DevOps, JFrog, Atlassian) with zeros in a stage (e.g., 0/15 on monitoring) should have that explicitly called out; Jenkins and CloudBees are not positioned as full platforms.
13. **New design-team competitive infographic** — William asks for the link; the team races to find it (William wins via Gmail folder search). The infographic is shared in chat; there are versions for single two-product comparisons and for platform players.
14. **Infographic architecture: data model vs. presentation** — The infographic is built on a data model separating data from presentation (stages can be renamed without rebuilding spaghetti code); v1 uses the 10 GitLab stages, and a future iteration will let users click into a stage to see the 15 features with value-based descriptions explaining what each stage actually is (addressing the "audience doesn't know what Verify/Manage means" problem).
15. **Infographic color scheme** — Design went green-only (no red/yellow) so the asset reads as a comparison ("less good" rather than "terrible"), which supports coopetition/partner messaging. William personally prefers a harder competitive tone with red/yellow but agrees to stick with green for now.
16. **Messaging framework — punchy tagline exercise** — William shares an assignment he received to fill in messaging "boxes" with punchy statements, and walks the team through draft taglines for three pillars:
    - Transparency/single source of truth: **"A single source of truth, countless possibilities"** (well-liked); "All-in-one for everyone" also floated as catchy.
    - End-to-end / factory control: **"End-to-end control over your software factory"** — debated; Cindy's earlier "secure the software factory and all the stuff you're making in it" phrase is borrowed and iterated on.
    - Speed + confidence/risk: candidates include "Move fast with confidence," "More speed, less risk," "Velocity with confidence," "More speed, less risk, high confidence," "No trade-offs" (rejected — engineering reality says there are always trade-offs; "less risk" / "mitigate risk" is preferred over "no risk").
17. **Talladega Nights / pop-culture detour & close** — Light banter about NASCAR, Ricky Bobby, *Talladega Nights* (assigned as weekend homework, to be discussed Monday on the social report), and *Step Brothers*; William settles on "More speed, less risk," notes the messaging deliverable is due end-of-day, and wraps the call.

---

## People Index

- **William**
  - Facilitates the meeting and opens with agenda.
  - Owns the corporate events follow-up and drives the GTM/campaign-manager commitment ask.
  - Finds and shares the new design-team competitive infographic (via a dedicated Gmail folder used for GitLab pings).
  - Owns the messaging framework assignment (tagline work); presents drafts, channels Ash Withers for catchy phrasing, and lands on "More speed, less risk."
  - Untags himself from (apparently) a messaging/action item at the close.
  - Recommends Assembly and assigns *Talladega Nights* as weekend "homework."
- **Cindy (Cynthia)**
  - Discusses event sign-ups; notes that if feedback is too thin she and William may just pick events themselves with others as backup.
  - Proposes grouping MVCs into thematic bundles (e.g., vulnerability management) rather than listing individual tiny features, and asks whether 14.0 items can be reused (confirmed yes).
  - Coined the "secure the software factory and its deliverables" phrasing that William borrows for messaging.
  - Gives feedback on tagline tone/parallelism ("more speed, less risk").
- **Tai**
  - Added/assigned people to the events issue ("Tai put in some folks").
- **Brian**
  - Added the product announcements topic to the agenda.
  - Walks through his approach to SCM announcements: grouping key iterations under umbrella themes (e.g., Getaway cluster bucket, research-based UX bucket) and assigning excitement levels; defends "UX" as a legitimate headline bucket.
  - Owns adding VS Code to the announcement list (has good notes on it, including community-contribution angle).
  - Gives feedback on the end-to-end-control and security taglines ("secure and control your software factory…").
  - Added (per thread) Atlassian back onto the competitor list after prior discussion.
- **Cormac**
  - Notes long-awaited Plan features like Epic Boards and Milestone Burn-up Charts as candidates; rates burn-up charts a "2" on excitement.
  - Is pinged by William to add a top-five Plan pick for the Commit announcements.
- **Harsh**
  - Responded to William's comment about why low-excitement items are included; suggested stack-ranking rather than a flat 1–3 score.
- **Samya**
  - Asks Elita for clarification on competitive-sheet scope (relevant competitors per stage vs. full Tier-1 list); is handling CD/Configure and Monitor competitive features and getting PM review.
- **Elita**
  - Answers Samya's competitive-sheet question: each sheet should only list Tier 1 competitors that apply to that stage; Tier 2/3 will come later; confirms a GitLab row should be added; confirms platform vendors with zero capabilities in a stage should have that called out.
- **Christie (with "Newb"/Nubu)**
  - Referenced in connection with the Commit product keynote, which the top-five announcement list is meant to feed.
- **Sid (Sid Sijbrandij)**
  - Mentioned as the keynote speaker at Commit alongside the product keynote; announcements are hoped to surface in Sid's keynote as well.
- **Ash Withers**
  - Referenced (not present) as the internal bar for catchy turns of phrase; William tries to channel him on messaging.
- **Parker**
  - Mentioned in jest during the "no trade-offs" / Ricky Bobby banter.
- **Corporate events team**
  - Does the operational "cat herding" for events; relies on PMMs/campaign managers to commit.
- **GTM teams / Campaign managers**
  - Each is expected to "sponsor" an event (sign up and run campaigns); PMMs need to obtain their explicit commitment on the issue.
- **PR team**
  - Consumer of the top-five announcement list; needs "fodder" for press around Commit.
- **Design team**
  - Produced the new competitive infographic (green-only, stage-based, data-driven).

---

## Keyword Index

- **13.0 / 14.0 (GitLab releases)** — Anchor releases used to bound the look-back window (since 13.0) and to reuse launch content (14.0 items) for Commit announcements.
- **ADO (Azure DevOps)** — Tier 1 competitor; positioned as a platform; called out where it has zero capabilities (e.g., monitoring).
- **All-in-one for everyone** — Catchy tagline candidate William likes; debated vs. more parallel phrasing.
- **Assembly** — Internal company event; William recommends watching it; framed as a rare company-wide look-back at wins (not shareable publicly).
- **Atlassian** — Tier 1 competitor/platform; Brian added back to the competitor sheet after discussion; still figuring out how to handle their positioning (partner vs. competitor tension).
- **Beta → GA** — Framing device for Commit announcements: features that entered beta in the last year and are now mature/ready to announce as GA.
- **"Browser" / Berserker (proprietary DAST scanner)** — GitLab's proprietary DAST scanner in beta for single-page applications; discussed as a possible security top-five pick alongside vulnerability management.
- **Brian's SCM bucketing approach** — Methodology for grouping key iterations under umbrella themes (Getaway cluster, research-based UX) with excitement ratings.
- **Campaign managers** — GTM-side owners who run event campaigns; PMMs need their explicit commitment (issue comment) for corporate events.
- **CD / Configure (stage)** — Stage Samya is working on for the competitive sheet; will have its own set of relevant competitors.
- **Christie & "Newb" keynote** — Product keynote at Commit that the announcement list feeds into (alongside Sid's keynote).
- **CI/CD (stage)** — Assigned to Google Next; Pipeline Editor and Value Stream Analytics discussed as possible top-five picks.
- **CloudBees** — Tier 1 competitor; not positioned as a full platform (along with Jenkins).
- **Commit (GitLab user event)** — Upcoming event where major product announcements and keynotes (product + Sid) will happen; primary driver of the product-announcements topic.
- **Community contributions** — Highlighted as a cool narrative: VS Code integrations and the Terraform module both started as community projects and became officially supported.
- **Competitive infographic (design team)** — New green-only, stage-based comparison asset built on a data/presentation separation; v1 shows stages, future v2 will drill into 15 features per stage with descriptions.
- **Competitive sheet / comparison spreadsheet** — PMM deliverable mapping stages to features and scoring Tier 1 competitors; tabs initially copy-pasted with the same five competitors across stages, to be trimmed to relevant ones.
- **Coopetition** — Rationale for the green-only color scheme (avoid calling partners "terrible" in red).
- **Cormac (Plan pick)** — Assigned to produce a top-five Plan entry for Commit (Epic Boards / Burn-up Charts discussed).
- **Create (stage)** — Discussed in the context of UX improvements and Brian's SCM key iterations; there are "at least a dozen" features that could be listed.
- **DAST (Dynamic Application Security Testing)** — Proprietary scanner category; "Browser"/Berserker is the SPA-focused beta.
- **Docs ↔ Spreadsheets workflow** — Acknowledged pattern of moving back and forth between Google Docs and Sheets; the announcement list may end up in a spreadsheet for stack-ranking.
- **Elita (competitive-sheet owner)** — Clarifies Tier-1-only-for-now policy and GitLab-row requirement for Samya.
- **End-to-end control over your software factory** — Tagline candidate for the platform/control pillar; criticized for not being parallel in tone with the other pillars.
- **Epic Boards** — Long-requested Plan feature, years in the making; top-five candidate.
- **Excitement levels (1–2–3)** — Scoring approach for announcement candidates; informed by MAU, upvotes, customer anecdotes; later constrained to a stack rank.
- **Fuzzing acquisition(s)** — Security story from roughly the prior spring/summer; already received press around acquisition and integration, considered "worn out" for another press hit at Commit.
- **Getaway / GitOps cluster (agent)** — Bucket of capabilities Brian grouped key iterations under; Kubernetes Agent + HashiCorp integrations rolled into a GitOps top-five candidate.
- **GitHub** — Tier 1 competitor positioned as a platform; should show zeroes in stages where it lacks capabilities.
- **GitLab row (competitive sheet)** — Required row so the asset reads honestly (shows GitLab gaps too, not just wins).
- **GitOps (team/event)** — Sync cadence cancelled after enablement; event sponsorship assigned to KubeCon.
- **Gmail folder trick** — William's personal productivity hack: routing all GitLab pings to a Gmail folder for fast searching.
- **Google Next** — Corporate event assigned to the CI/CD GTM team.
- **GTM (Go-To-Market) teams** — New event-sponsorship model: each GTM team signs up to "sponsor" an event, supported by their PMM and campaign manager.
- **Harsh (stack-rank feedback)** — Suggested constraining 1–3 ratings to a stack rank instead of independent scores.
- **HashiCorp integrations** — Bundled with Kubernetes Agent into a GitOps top-five candidate.
- **Incident management** — Example Brian used of bucketing multiple Monitor releases under one umbrella (noted that core incident management shipped as part of trial.x / 13.x era).
- **Infographic colors (green-only vs. red/yellow)** — Design chose green-only to read as a comparison, not a takedown; William prefers red/yellow competitively but defers for now.
- **Integrations (stage/theme)** — Called out as under-represented but potentially exciting; Jira and VS Code cited as notable.
- **Issue (corporate events)** — Single source of truth for event sponsorship sign-ups; PMMs asked to get their campaign managers to comment with a commitment.
- **JFrog** — Tier 1 competitor/platform; zeroes to be called out where applicable.
- **Jenkins** — Tier 1 competitor; not positioned as a full platform (along with CloudBees).
- **Jira integration** — Notable integration candidate worth highlighting for Commit.
- **Key iterations** — Brian's term for specific features rolled up under umbrella themes in the announcement list.
- **KubeCon** — Corporate event assigned to the GitOps GTM team; used as the example of asking campaign managers for a commitment comment.
- **Kubernetes Agent (ks agent)** — Significant GitOps investment; bundled with HashiCorp integrations as a top-five candidate.
- **Logs** — Segment William says they will likely not highlight even though they want to show the full spectrum of features.
- **Manage (stage)** — Initially missing from the announcement list; added after review; noted as less descriptive than ideal naming.
- **MAU (Monthly Active Users)** — Proposed quantitative signal for excitement levels; acknowledged as unavailable for all features and requiring benchmarks.
- **Messaging framework / boxes** — Assignment William received to write punchy statements across three pillars (transparency, control, speed/risk).
- **Milestone Burn-up Charts** — Plan feature; Cormac rates it a 2 in excitement and notes it can be bundled with other Plan items.
- **Monitor (stage)** — Stage Samya is working on for the competitive sheet; example of a stage where many Tier 1 competitors (e.g., CloudBees) do not apply; Brian applied the same key-iterations bucketing approach for incident management here.
- **"More speed, less risk"** — Final tagline selected for the speed/security pillar; preferred over "move fast with confidence," "no trade-offs," and "more speed less risk with confidence."
- **Move fast with confidence** — Popular tagline candidate; rejected partly because it breaks grammatical parallelism with noun-phrase pillar statements.
- **MVC (Minimum Viable Change)** — Core GitLab shipping philosophy; creates the problem this exercise solves (features ship incomplete and mature over time, so you have to look back to find announceable wins).
- **"Newb" (Nubu?)** — Co-presenter with Christie on the Commit product keynote; name unclear from audio.
- **"No trade-offs"** — Catchy candidate tagline rejected by the engineering-minded group because there are always trade-offs; "less risk" / "mitigate" preferred over "no risk."
- **Pipeline Editor** — CI/CD feature floated as a top-five candidate.
- **Plan (stage)** — Needs a top-five entry (Cormac pinged to add); Epic Boards and Milestone Burn-up Charts are candidate items.
- **Platform players (competitors)** — GitHub, Azure DevOps, JFrog, Atlassian — competitors that position themselves as full platforms; zeros across a stage should be explicitly called out. Jenkins/CloudBees are excluded from this group.
- **PR team** — Audience for the top-five list; will use it to drive press around Commit.
- **Proprietary scanning** — Security bucket discussed (Semgrep replacement, proprietary DAST/Browser); ultimately vulnerability management chosen over it for top-five.
- **re:Invent (AWS re:Invent)** — Corporate event assigned to the Platform GTM team.
- **Research-based UX** — Umbrella bucket Brian used for key iterations representing UX improvements; cited as a standard headline move (e.g., Windows 11 launches lead with UX).
- **Ricky Bobby / Talladega Nights / Step Brothers** — Pop-culture detour; *Talladega Nights* assigned as weekend homework, to be discussed on Monday's social report; Parker referenced in banter.
- **"Secure the software factory and its deliverables"** — Phrase William borrowed from Cindy's 14.0 blog post; used as a starting point for the security/control tagline.
- **Semgrep** — Open-source scanner GitLab replaced with one of its own; discussed but not chosen as the lead security announcement.
- **Sid's keynote** — CEO keynote at Commit; the team hopes top-five announcements can be referenced there as well as in the product keynote.
- **"Single source of truth, countless possibilities"** — Preferred tagline for the transparency pillar.
- **Slack thread (events)** — Where event assignments were initially farmed out before being tracked in the issue.
- **Slack syncs / regular cadence** — ~2-week cadence for most teams; GitOps sync cancelled after enablement.
- **Social report (Monday)** — Weekly social chat where the team will discuss *Talladega Nights*.
- **Spaghetti code** — William's descriptor for the old infographic/codebase that made stage-name changes hard; contrasted with the new data/presentation separation.
- **Stack rank** — Harsh/William's proposal to constrain excitement ratings (e.g., only one "3" per PMM) so items can be meaningfully ranked across stages.
- **Terraform module/integration** — Community-contributed module that became officially supported; bundled into the GitOps top-five candidate with Kubernetes Agent; a strong "yes you can use this in production" signal for enterprise customers.
- **Tier 1 / Tier 2 / Tier 3 competitors** — Competitor prioritization scheme; current competitive-sheet work is Tier 1 only; Tier 2 and 3 will be added later.
- **Top-five overall** — Cross-stage, PR-ready shortlist of announcement themes for Commit: UX, Vulnerability Management, GitOps (K8s Agent + HashiCorp/Terraform), CI/CD (Pipeline Editor / VSA), Plan (TBD — Cormac).
- **Tuesday deadline** — Due date for the Commit product-announcements list.
- **Two-week cadence** — Current sync cadence across teams (down from more frequent).
- **UX improvements (as a headline bucket)** — Endorsed as a legitimate top-line announcement theme; cross-stage, includes more than just Create, and mirrors how other companies (e.g., Windows 11) lead launches.
- **Value Stream Analytics (VSA)** — Mentioned as a possible CI/CD story; customizable VSA shipped in 12.9, which the team regrets (just outside the ideal window).
- **Verify (stage)** — Used as an example of a stage name that doesn't mean much to outsiders; future infographic iteration will add explanatory descriptions when clicking into a stage.
- **Vulnerability management** — Chosen as the lead security announcement for Commit; preferred over fuzzing (already press-worn) and proprietary scanners; aggregates many small MVCs across the year.
- **VS Code integrations** — Two community-contributed VS Code extensions/integrations landed in the same month; one long-standing extension became official; Brian volunteered to add them; cool community-story angle.
- **William's messaging pillars** — Three buckets to be filled with punchy taglines: (1) transparency/source of truth, (2) end-to-end control/security, (3) speed + confidence/risk.
- **Windows 11** — Referenced as an example of a product launch that leads with UX as one of its top headlines, validating Brian's UX bucket.

---

## Decisions Log

1. **(Topic: Corporate events) Event sponsorship model** — Each GTM team "sponsors" an event: the PMM supports it and the GTM campaign manager runs the campaigns for it. PMMs are responsible for getting an explicit commitment (comment on the issue) from their campaign manager.
2. **(Topic: Corporate events) Event-to-team assignments** — Platform → AWS re:Invent; CI/CD → Google Next; GitOps → KubeCon.
3. **(Topic: Product announcements) Announcement framing** — Commit announcements will look back ~1 year (post-13.0) at features that shipped as MVCs and have since matured / exited beta, reusing 14.0 launch material where useful and adding more items to hit different press areas.
4. **(Topic: Product announcements) Thematic grouping over individual MVCs** — Small MVCs will be rolled up into umbrella themes/"key iterations" (e.g., vulnerability management, UX, GitOps cluster) rather than listed individually; UX is an acceptable and encouraged headline bucket.
5. **(Topic: Product announcements) Target size** — Aim for roughly three items per stage.
6. **(Topic: Product announcements) Include Manage and Integrations** — Manage stage must be added; Integrations (Jira, VS Code) should be included.
7. **(Topic: Product announcements) Excitement scoring → Stack rank → Top-five** — Excitement levels (1–3) will be used, but constrained into a stack rank rather than independent scores, and the team will ultimately produce a cross-stage "top five overall" list for PR/keynotes.
8. **(Topic: Product announcements) Top-five shortlist (directional)** — UX (cross-stage) is a lock; security pick is Vulnerability Management (fuzzing is worn out, proprietary scanners like Semgrep/DAST are secondary); GitOps bundle is Kubernetes Agent + HashiCorp/Terraform integrations; CI/CD slot is Pipeline Editor (with VSA as a possible angle); Plan slot will be provided by Cormac.
9. **(Topic: Product announcements) Deadline** — The announcement list is due Tuesday.
10. **(Topic: Competitive sheet) Scope = Tier 1 competitors, relevant per stage** — Only Tier 1 competitors applicable to a given stage need to be filled out on that stage's tab; Tier 2/3 will follow later. Tabs should not carry the copy-pasted full competitor list if competitors don't apply.
11. **(Topic: Competitive sheet) Add a GitLab row** — Each stage tab must include GitLab so the comparison reads honestly (showing both wins and gaps).
12. **(Topic: Competitive sheet) Market-lens framing** — Features should be chosen from a market/buyer lens; not all GitLab-only; a mix of GitLab-leading, GitLab-behind, and competitor-leading items makes the asset trustworthy.
13. **(Topic: Competitive sheet) Call out platform-vendor zeros** — Platform vendors (GitHub, ADO, JFrog, Atlassian) that have zero capabilities in a stage should be explicitly marked 0/15 (e.g., monitoring); Jenkins and CloudBees are not treated as full platforms.
14. **(Topic: Competitive sheet) Atlassian stays** — Atlassian will remain on the competitor sheet (treated as a competitor/platform, not just a partner).
15. **(Topic: Competitive infographic) Stage-based v1; deeper feature drill-down v2** — V1 of the new infographic uses GitLab's 10 stages as the top-level view (built on a data model separating presentation from data so labels can change); a future iteration will let users click into a stage to see the ~15 features with value-based descriptions of what each stage means.
16. **(Topic: Competitive infographic) Color scheme stays green-only for now** — Despite William's personal preference for red/yellow, the green-only palette (no "worse" red cells) will ship as-is to keep the tone cooperative/industry-helpful rather than attack-ad; subject to revisit later.
17. **(Topic: Messaging framework) Pillar 1 tagline** — "A single source of truth, countless possibilities" is the preferred line for the transparency/source-of-truth pillar.
18. **(Topic: Messaging framework) Pillar 3 tagline** — "More speed, less risk" is selected for the speed/risk/confidence pillar; "no trade-offs," "move fast with confidence," and "more speed less risk with confidence" are rejected (engineering accuracy and parallelism concerns).
19. **(Topic: Messaging framework) Tone parallelism** — Pillar statements should match each other grammatically (noun phrases for noun-phrase pillars, not mixed imperative/noun styles), which informs the choice between candidates.

---

## Action Items Log

| # | Action Item | Owner | Deadline / Timing |
|---|---|---|---|
| 1 | Get campaign managers to explicitly commit (comment on the issue) to their assigned event (re:Invent, Google Next, KubeCon) so corporate events can plan. | Each PMM (working with their GTM team) | Not specified — implied ASAP / before event planning locks |
| 2 | Continue GTM-team outreach on Slack and the issue for event commitments; if insufficient response, William + Cindy will pick events themselves with others as backup. | William & Cindy (fallback) | Not specified |
| 3 | Add VS Code integrations (including community-contribution angle) to the Commit product announcement list. | Brian | By Tuesday (announcement-list deadline) |
| 4 | Add a top-five Plan item (e.g., Epic Boards, Milestone Burn-up Charts) to the Commit announcement shortlist. | Cormac (William to ping) | By Tuesday |
| 5 | Aggregate the full list of shipped features over the last ~12 months, re-bucket into themes, and identify the final top-five overall for PR/keynotes. | William (with PMM input) | By Tuesday |
| 6 | Time-boxed review of the top-five candidate list during/after the meeting. | Whole team | During the call (5-minute pass) |
| 7 | For the competitive sheet: on each stage tab, keep only the Tier 1 competitors relevant to that stage (strip out ADO/Atlassian/GitHub/Jenkins/JFrog/CloudBees where they don't apply); leave non-applicable Tier-1 cells blank for now; Tier 2/3 to be added later. | Each PMM (e.g., Samya for CD/Configure & Monitor) | Not specified |
| 8 | Add a GitLab row to each stage tab of the competitive sheet, filled out honestly (wins + gaps) through a market lens. | Each PMM | Not specified |
| 9 | For platform vendors (GitHub, Azure DevOps, JFrog, Atlassian) with zero capabilities in a given stage, explicitly call out 0/15 (e.g., on monitoring, configure). | Each PMM | Not specified |
| 10 | Review the new design-team competitive infographic (link shared in chat); use value-based stage descriptions in the v2 drill-down rather than just stage names like "Verify"/"Manage." | PMM team + design team (v2) | Future iteration (not immediate) |
| 11 | Share link to the new competitive infographic in the meeting chat/notes. | William (found it first via Gmail search) | Done during the call |
| 12 | Finalize the messaging framework taglines and submit the filled-in "boxes." | William | End of day (day of meeting) |
| 13 | Watch *Talladega Nights* as "homework"; discuss on Monday's social report. | Whole team | Weekend / Monday social report |
| 14 | Next-movie follow-up: *Step Brothers* (mentioned as a future watch). | Whole team | "Next week" (lighthearted) |

---

*Index generated from the transcript dated 2021-06-28. Quoted phrases are taken verbatim from the transcript where useful for search.*
