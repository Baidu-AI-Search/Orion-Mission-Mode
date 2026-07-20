# CSMAC May 30, 2012 Meeting — Technical Discussions Report

**Meeting:** Commerce Spectrum Management Advisory Committee (CSMAC)
**Date:** Wednesday, May 30, 2012
**Topic:** Facilitating commercial wireless broadband sharing with federal spectrum operations in the 1755–1850 MHz band (and 1695–1710 MHz)
**Source:** Meeting transcript, `meeting-transcript.md`

---

## 1. Overview of the 1755–1850 MHz Band Context

- NTIA completed a "fast track" report identifying 115 MHz of spectrum that could be made available on a shared basis (1695–1710 MHz, 1710–1755 MHz, and 3550–3650 MHz), using exclusion areas.
- The 1755–1850 MHz band was selected as the next band for study because it represents the last major piece of sub-3 GHz spectrum where relocation/redesign of major radar or radio-navigation systems is not required.
- The band contains over **3,100 federal assignments** across at least eight distinct federal applications. Most federal operations span the entire band, many operate intermittently, and many are airborne (UAVs, precision-guided munitions, air combat training systems, telemetry).
- Federal agencies indicated they could move almost all systems out of the band within **10 years**, except satellite uplinks and electronic warfare training, which must remain. Initial cost estimate for relocation is approximately **$18 billion**. The target band for relocation is **2025–2110 MHz** (government satellite uplinks and electronic news gathering).
- Because of cost, transition time, and ongoing operational needs, NTIA proposed a cooperative government/industry effort to develop **sharing** approaches rather than pure relocation.

---

## 2. The Five Proposed Working Groups

NTIA proposed five working groups — one on 1695–1710 MHz and four on 1755–1850 MHz — co-chaired by one government and one industry representative, with FCC and NTIA representatives assigned to each, and a CSMAC member serving as liaison to the main body. The existing 500 MHz and Sharing Subcommittees were placed on temporary hiatus to focus resources.

### Working Group 1 — Weather Satellite Receivers (1695–1710 MHz)
- **Scope:** Re-evaluate exclusion/coordination zones around weather satellite earth stations. The fast-track report defined exclusion areas based on an assumed commercial deployment environment; the group is to test whether that assumption was accurate and whether zones can be shrunk or replaced with alternative approaches.
- **Government co-chair:** Yvonne Navarro, Department of Commerce / NOAA.
- **NTIA participant:** Ed Drocella.
- **Target completion:** Earlier than the other groups — **September 2012** — to support near-term re-purposing.
- **Frequency band:** 1695–1710 MHz.

### Working Group 2 — Law Enforcement Surveillance, Electronic Ordnance Disposal, and Short-Distance Links
- **Scope:** Address compatibility of low-power, wide-bandwidth law enforcement surveillance devices and related short-range federal links with ubiquitous commercial wireless deployment. These systems operate across the entire 1755–1850 MHz band, are not compatible with commercial broadband, and have nationwide, on-demand authorization. The group is to develop a phased transition plan (city-by-city / market-by-market), aligning industry build-out priorities ("NFL cities") with federal vacate schedules.
- **Government co-chair:** A representative from DHS/DOJ (name pending at time of meeting); NTIA support from Rich Orsulak and Scott Jackson (public-safety background).
- **Federal three-step transition already outlined:** (1) vacate 1755–1780 MHz within 5 years while migrating to digital technology; (2) later compress into smaller bandwidth via improved digital capability; (3) potentially move out of the band entirely if a suitable replacement band is identified.
- **Frequency band:** 1755–1850 MHz (entire band, with priority focus on 1755–1780 MHz first).

### Working Group 3 — Satellite Control Links and Electronic Warfare Training
- **Scope:** Two distinct federal operations that cannot be relocated in the near term:
  - **Satellite control (uplink):** Earth station uplinks transmit to satellites; interference direction is unique — *commercial receivers are at risk of interference from federal uplinks* (rather than the other way around). The group must define a regulatory construct that allows maximum commercial use while guaranteeing federal uplinks are protected.
  - **Electronic warfare (EW) training:** DoD must be able to test/train against real-world commercial-type signals (including cell-phone-triggered devices). The group must define regulatory protections that guarantee training access even after commercial systems are deployed.
- **Government co-chair:** Not named at the meeting (DoD-led).
- **Frequency band:** 1755–1850 MHz (satellite operations cannot be altered below 1780 MHz).

### Working Group 4 — Tactical Radio Relay and Fixed Microwave
- **Scope:** Evaluate sharing with federal fixed microwave links and tactical radio relay. Fixed microwave is familiar from the 1710–1755 MHz relocation, and Gary Patrick (who led that relocation) is assigned. Tactical radio relay has different characteristics but prior experience suggests exclusion zones around the three permanent sites can be narrowed through on-the-ground coordination. The group will also explore convergence opportunities as DoD moves toward more commercial-type technologies.
- **Government co-chair:** Not named at the meeting; Gary Patrick (NTIA) leads fixed-microwave work.
- **Frequency band:** 1755–1850 MHz.

### Working Group 5 — Airborne Operations
- **Scope:** Address the most technically challenging federal systems — airborne platforms including UAVs, precision-guided munitions, air combat training systems, and telemetry. The airborne nature creates complex, geometry-dependent interference problems (mismatch of bandwidths, timing, and distance). The group will rely heavily on empirical measurements and will work with the T-Mobile/CTIA Special Temporary Authority (STA) testing.
- **Government lead/participant:** John Hunter (DoD detailee, formerly T-Mobile).
- **Frequency band:** 1755–1850 MHz (entire band).
- **Target completion:** January 2013 (along with WGs 2–4), to align with the 2155–2180 MHz pairing for commercial auction.

---

## 3. Technical Topics Extracted from the Discussion

### Topic 1 — Exclusion Zone Refinement for Weather Satellite Earth Stations (1695–1710 MHz)
- **Topic title:** Re-sizing weather satellite receiver exclusion/coordination areas
- **Frequency band(s) involved:** 1695–1710 MHz
- **Federal systems/applications affected:** NOAA weather satellite earth station receivers (satellite downlink reception)
- **Technical challenge described:** The fast-track report defined exclusion areas around earth stations based on an *assumed* commercial deployment environment. If that assumed environment overstated commercial power levels or used unrealistic worst-case parameters, the resulting exclusion areas may be larger than necessary, reducing the commercial utility of the band. If it understated the commercial footprint, federal receivers could be harmed.
- **Proposed approach or solution:** Re-examine the commercial environment assumption, model more realistic deployments, and refine (shrink) exclusion/coordination areas using improved parameters; consider alternative protection approaches beyond static exclusion zones.
- **Working group assignment:** **Working Group 1** (Weather Satellite Receivers); Ed Drocella (NTIA) and Yvonne Navarro (NOAA) involved.

### Topic 2 — Low-Power Wideband Law Enforcement Surveillance Compatibility
- **Topic title:** Coexistence and phased transition for low-power law enforcement surveillance and EOD links
- **Frequency band(s) involved:** 1755–1850 MHz (initial vacate of 1755–1780 MHz first)
- **Federal systems/applications affected:** Law enforcement surveillance systems, electronic ordnance disposal (EOD) devices, short-distance federal links. These are mostly low-power, very-wide-bandwidth receivers used nationwide on an as-needed basis.
- **Technical challenge described:** Wide-bandwidth receivers mean a single commercial broadband signal can disrupt multiple surveillance operations simultaneously. Systems have nationwide authorization (operate wherever/whenever needed), are not compatible with ubiquitous commercial LTE-like deployments, and this incompatibility was the "biggest hurdle" in the prior 1710–1755 MHz relocation. Sensitive operational details historically limited pre-auction sharing analysis.
- **Proposed approach or solution:** Execute the federal three-step transition plan: (1) vacate 1755–1780 MHz within ~5 years and migrate to digital technology; (2) further compress into smaller bandwidth via improved digital capability; (3) eventually vacate entirely if a suitable replacement band is found. Transition is city-by-city/market-by-market; the group will align federal vacate sequencing with industry "NFL city" build-out priorities.
- **Working group assignment:** **Working Group 2** (Law Enforcement Surveillance / EOD / Short-Distance Links); supported by Rich Orsulak and Scott Jackson (NTIA).

### Topic 3 — Satellite Control Uplink Interference into Commercial Receivers
- **Topic title:** Reverse-direction interference protection for satellite control uplinks
- **Frequency band(s) involved:** 1755–1850 MHz (federal satellite operations cannot be modified below 1780 MHz)
- **Federal systems/applications affected:** Federal satellite control uplinks (earth stations transmitting to government satellites)
- **Technical challenge described:** Unlike the more common case of commercial transmitters interfering with federal receivers, here federal *uplink* transmitters can interfere with commercial *base-station/user* receivers. Satellite control links must remain in place for the foreseeable future and do not fit a traditional exclusion-zone model because the harmful-interference direction is inverted.
- **Proposed approach or solution:** Develop a regulatory construct (e.g., operating rules, licensing conditions, power/coordination requirements) that permits commercial use across the band while guaranteeing federal satellite uplinks can operate without causing harmful interference to commercial networks and without their own operations being constrained. This is less a propagation-modeling problem and more a regulatory/protection-framework problem.
- **Working group assignment:** **Working Group 3** (Satellite Control and Electronic Warfare).

### Topic 4 — Electronic Warfare Training Access in a Commercially Occupied Band
- **Topic title:** Ensuring DoD electronic warfare training access after commercial deployment
- **Frequency band(s) involved:** 1755–1850 MHz
- **Federal systems/applications affected:** DoD electronic warfare (EW) test and training ranges, including training against commercial-technology threats (e.g., cell-phone-triggered improvised devices)
- **Technical challenge described:** EW training requires access to a band populated with live commercial-like signals to be realistic, but DoD must be able to operate jamming/test transmitters without permanently disrupting commercial networks. Training is time- and location-critical and must be guaranteed post-deployment.
- **Proposed approach or solution:** Define a regulatory construct (e.g., scheduling/coordination windows, geographic training corridors, notification procedures) that guarantees DoD access to the band for realistic training while protecting commercial operations.
- **Working group assignment:** **Working Group 3** (Satellite Control and Electronic Warfare).

### Topic 5 — Tactical Radio Relay and Fixed Microwave Coordination
- **Topic title:** Narrowing protection areas around tactical radio relay and fixed microwave sites
- **Frequency band(s) involved:** 1755–1850 MHz
- **Federal systems/applications affected:** Federal fixed microwave links and military tactical radio relay, including three permanent tactical radio relay sites
- **Technical challenge described:** Initial worst-case/sharing calculations tend to produce very large protection areas around these sites. Experience from 1710–1755 MHz shows that, with site-specific coordination and engineering, real-world protection areas can be much smaller than the initial modeled contours.
- **Proposed approach or solution:** Apply lessons from prior relocations; use on-the-ground, site-specific coordination to shrink exclusion zones; explore opportunities as DoD moves toward commercial-type technologies, potentially enabling more seamless coexistence and training opportunities.
- **Working group assignment:** **Working Group 4** (Tactical Radio Relay and Fixed Microwave); led in part by Gary Patrick (NTIA).

### Topic 6 — Airborne Systems (UAVs, Telemetry, Air Combat Training, PGMs)
- **Topic title:** Sharing with high-dynamic-range airborne federal operations
- **Frequency band(s) involved:** 1755–1850 MHz (entire band; most airborne systems operate across it)
- **Federal systems/applications affected:** Unmanned aerial vehicles (UAVs), precision-guided munitions (PGMs), air combat training systems, flight telemetry
- **Technical challenge described:** These are intermittent, airborne, high-power/mobility systems with very different emission characteristics (bandwidth, duty cycle/timing, altitude/distance) from terrestrial systems. Key analysis issues are (a) bandwidth mismatch between commercial channels and airborne emitters, (b) timing/duty-cycle behavior, and (c) distance/geometry. Conventional worst-case, max-power simulations are unrealistic and lead to overly pessimistic results. Interference runs in both directions — airborne emitters can affect commercial networks, and commercial deployments can affect airborne receivers.
- **Proposed approach or solution:** Conduct empirical measurements (including the T-Mobile/CTIA STA tests) to characterize actual airborne emission activity (how often devices operate, what they look like); leverage the expectation that modern commercial technologies (LTE and successors) are more interference-tolerant than legacy systems; develop realistic (rather than worst-case) deployment and interference models; use measurements to determine whether commercial systems can tolerate actual airborne emitter environments.
- **Working group assignment:** **Working Group 5** (Airborne Operations); John Hunter (DoD/NTIA) leading.

---

## 4. Key Technical Debate — Commercial Deployment Parameters for Sharing Analysis

A major cross-cutting debate, raised most forcefully by Charles Rush (seconded by Janice Obuchowski, Jennifer Warren, Michael Calabrese, and Mark Gibson), centered on **what set of commercial deployment parameters should be used as the common baseline for sharing studies across all four 1755–1850 MHz working groups**.

### Core concern
If each working group (or each company) uses its own assumptions about how commercial networks will be deployed, the resulting analyses will produce inconsistent, incompatible conclusions — leading to "dissected" viewpoints and recommendations that cannot be reconciled at the NTIA/FCC level.

### Parameters in dispute
- **Base station and user equipment power levels** (max EIRP vs. typical deployed power)
- **Antenna heights, patterns, and downtilt**
- **Cell architecture:** whether the model should assume traditional macrocells only, or a heterogeneous mix of **macrocells + microcells + picocells + femtocells + relays**, with small cells embedded inside macro coverage (as is increasingly the real deployment model)
- **Duty cycle / loading**
- **Frequency re-use factors**
- **Receiver characteristics** (especially interference tolerance and linearity of LTE/4G devices, which are expected to be more robust than legacy systems)
- **Technology baseline:** which 3GPP/LTE family release(s) and roadmap over the 10-year horizon

### Why the parameters matter
- Worst-case, max-power, macrocell-only simulations produce the largest exclusion zones and most pessimistic sharing results — and, in Charlie Rush's words, represent "worst-case, impossible, totally unrealistic simulations."
- Realistic heterogeneous-network (HetNet) modeling (with embedded picocells/femtocells/relays, lower power nodes, and real-world loading) can dramatically shrink apparent interference impacts and show that sharing is more feasible than worst-case analyses suggest.
- The choice of parameters directly affects federal protection-area sizes, transition timing, auction viability, and ultimately how much commercial capacity the band can deliver.
- Janice Obuchowski noted a "substantial difference of opinion" between the **PCAST study** view (emphasizing small-cell, shared-use models) and the **traditional carrier macrocell** view on which auction revenues have historically been based; the FCC had not yet signaled which architecture it would adopt for the commercial band plan, creating policy risk for the working groups' technical outputs.

### Positions and proposed way forward
- **Charles Rush** (with support from Michael Calabrese, Jennifer Warren) advocated explicitly recreating the successful model used at the FCC around the year 2000 for IMT-2000: ask industry to collectively submit a shared, documented set of deployment parameters and values (analogous to the process that produced the ITU-R IMT-2000 parameter sets and TSB-10–style sharing guidance); the alternative — NTIA setting parameters unilaterally — would be less credible.
- **Kevin Kahn (Intel)** argued the problem is somewhat bounded because the commercial standards trajectory is already largely coalesced around the **LTE family**, with fairly predictable roadmaps over the 10-year horizon; while "arguments around the corners" will persist, the dominant thrust is unlikely to change dramatically.
- **Bryan Tramont** cautioned that full ex-ante agreement across all commercial segments (CMRS providers with different models, unlicensed providers, manufacturers) might not be achievable, and emphasized consistent representation across the five working groups and cross-group communication rather than a separate parameter-definition track.
- **Michael Calabrese** urged that small-cell/low-power scenarios be explicitly evaluated, because high-power-only models (as in the original fast-track analysis) can miss important policy insights about efficient sharing.
- **Mark Gibson** observed that sharing paradigms have historically been codified into guidance documents (e.g., NTIA Bulletin 10, TIA TSB-10F) and urged that TIA and similar standards bodies be included so that parameter sets get memorialized in shareable engineering tools and software.
- **Karl Nebbia (NTIA)** committed that industry must coalesce on a common commercial characterization; did not commit to creating a separate parameter-definition working group immediately, but noted (a) WG3 (satellite/EW) could begin its regulatory-construct work in parallel, and (b) WG5 (airborne) can begin by making measurements even before the full commercial parameter set is finalized.
- **Chair Fontes** acknowledged the validity of the issue and committed to following up so that conflicting input assumptions do not produce conflicting reports.

---

## 5. Specific Technical Measurements and Tests Mentioned

### T-Mobile / CTIA Special Temporary Authority (STA)
- **Parties:** T-Mobile, working in coordination with **CTIA** (the wireless industry association).
- **Purpose:** To conduct **spectrum measurements and testing in the 1755–1850 MHz band** to help characterize the actual impact and environment created by federal airborne operations.
- **Activities described:**
  - Pure **spectrum monitoring/passive measurements** to observe federal emissions — e.g., how often the airborne devices operate, their signal characteristics (bandwidth, timing, power), and geographic patterns.
  - Potential **active testing in cooperation with government operations** to measure real-world interactions between commercial-like signals and federal systems.
- **Relevance to working groups:** Outputs will be a primary input to **Working Group 5 (Airborne Operations)** and will inform the assessment of whether modern commercial (LTE) receivers can tolerate actual airborne-emitter environments. Results will also be important for calibrating parameters used in the broader commercial-deployment debate (Section 4).
- **Context:** Mentioned by Karl Nebbia alongside the announcement that John Hunter (DoD, formerly T-Mobile) has been brought on to NTIA to support airborne-operations work, underscoring that this test program is expected to be a major empirical foundation for the sharing analysis.

### Other measurement/characterization notes
- Prior experience with tactical radio relay sites showed that initial modeled protection contours were larger than necessary; real-world, site-specific measurements and coordination (analogous to the approach planned for WG4) were able to shrink these areas, in some cases allowing usable commercial signal "almost immediately outside the base."
- NTIA noted that, for law enforcement surveillance, some previously sensitive operational characteristics are now widely observable via routine spectrum monitoring, reducing — though not eliminating — information-sharing barriers.
- There was general consensus that modern commercial technologies (LTE and beyond) are more interference-tolerant than past generations, and that this hypothesis should be verified through testing/measurement as part of the WG5 effort.

---

## 6. Schedule and Deliverables Summary

| Item | Date / Target |
|---|---|
| WG1 (1695–1710 MHz weather satellites) completion target | **September 2012** |
| WGs 2–5 (1755–1850 MHz) completion target | **January 2013** |
| Next CSMAC meeting | July 24, 2012 (Boulder, CO; afternoon), followed by ISART meeting at ITS on spectrum sharing |
| Pairing band for commercial auction | 2155–2180 MHz (AWS-3 pairing consideration) |
| Decision framework | Consensus-based reports from each WG → reviewed by CSMAC → recommended to NTIA → provided to FCC for rulemaking/auction |

---

*Report compiled from the May 30, 2012 CSMAC meeting transcript. All quotes and factual references are drawn directly from that document.*
