# Niche Validation and Bible Build

A single end-to-end process: **3 candidate niches in → 1 winning niche out → a polished Niche Bible .docx for the member.**

---

## How to Run This

This SOP is built to be run in **Claude Code**.

**One-time setup (5 minutes):**

```bash
# 1. Install the market-researcher subagent (used in Stage 2)
mkdir -p ~/.claude/agents
cp market-researcher.md ~/.claude/agents/

# 2. Install the Python library used by the Stage 4 converter
pip3 install python-docx
```

**To run the workflow:**

Open this folder in Claude Code and type:

> *"Read niche_validation_sop.md and run me through it. Start by asking me for my 3 candidate niches."*

Claude Code will read the SOP, ask you for inputs at each stage, run the validation prompt, invoke the market-researcher subagent for the deep dive, walk you through the Ad Library screenshot step, compile the niche bible, and hand you back a polished `.docx`.

End-to-end: ~30 minutes, mostly hands-off.

---

## What to Expect When You Run This

This workflow takes ~30 minutes and produces one polished Word document — your **Niche Bible** — for the niche you commit to. Along the way, Claude Code will:

1. **Ask you for 3 candidate niches** you're considering. (Anything that's a real business — garage doors, window replacement, pool service, etc.)
2. **Score each one across 8 dimensions** of operational fit (~5 min). You'll see at a glance where each niche is strong and where it'll need extra effort.
3. **Run a deep-dive market research report** on your top niche — or both top 2 if you're undecided (~10 min). This is real industry data, not vibes: market size, average ticket, who the competitors are, where leads come from, what creative angles are winning.
4. **Pause and ask you to commit to one niche.** All three of your candidates can work — the assessment helps you see the operational fit, but the decision is yours. Personal interest, geographic match, and existing experience are valid factors alongside the data.
5. **Walk you through the Facebook Ad Library** for the niche you commit to (~5 min). This is where you do the most active work — opening Meta's free ad library, scrolling, screenshotting 5–10 ads with clear offers, and dropping them in the chat. This step does double duty: it confirms real businesses are spending money in your niche (so the channel is alive), and it gives Claude Code the raw material to build your offer bank.
6. **Build a 15-angle offer bank** from your screenshots plus industry-standard angles. This is the menu of headlines, hooks, and offers you'll use to write ads.
7. **Compile everything into a Niche Bible Word document** and save it to your folder.

You'll be prompted at each stage. The data and writing happen in the background — your job is to provide inputs, read the assessment when it arrives, and commit to a niche when ready.

> **Why split it into stages?** Each stage gates on the one before. The deep dive is wasted effort if the niche fails basic ticket math. The offer bank is wasted effort if the niche channel turns out dead. Compiling the .docx is wasted effort if you haven't actually decided which niche you're committing to. Doing them in sequence saves you from a polished document for a niche you'd never pick.

---

## Objective

Narrow three candidate niches down to one with rigorous validation, deep research, and live channel evidence — then compile everything into a single member-facing **Niche Bible** that contains the validation rationale, the full business-model deep dive, and a ready-to-use offer bank.

The bible is the deliverable. The member submits it back as their committed niche; Sam's team uses it to build the campaign.

---

## Process Overview

| Stage | Input | Output | Who runs it |
|---|---|---|---|
| **1. Assess** | 3 candidate niches | Ranked 1–3 with STRONG/MODERATE/WEAKER scoring across 8 dimensions + fit summary | AI (uses [niche-validation-prompt.md](niche-validation-prompt.md)) |
| **2. Deep Dive** | 1 winning niche, OR top 2 if member is undecided | 7-section markdown deep dive(s) with sourced industry data | `market-researcher` subagent |
| **— Decision —** | Stage 1 + Stage 2 outputs | **Member commits to ONE niche** | Member |
| **3. Offer Bank** | Committed niche name | 5–10 FB ad screenshots → 15-angle offer bank markdown | Member screenshots, AI builds |
| **4. Compile** | Stage 1 + 2 + 3 outputs for the committed niche | Single Niche Bible `.docx` | AI runs [tools/build_offer_bank_docx.py](tools/build_offer_bank_docx.py) on a stitched master markdown |

End-to-end time: ~30 minutes (5 min member input + ~25 min AI work). Add ~10 min if running comparison deep dives in Stage 2.

> **Critical sequencing rule:** the `.docx` is the final crystallization. **Do not generate it until a niche is committed AND its offer bank exists.** Stage 2 outputs are markdown only — comparison artifacts, not deliverables. If the member is undecided after Stage 2, run a second deep dive on the runner-up rather than skipping ahead.

---

## Stage 1 — Assess the Three Niches

### What's happening
You give Claude Code three niches you're considering. Claude Code runs each one through a structured assessment that scores 8 operational dimensions — things like ticket size, market depth, agency saturation, owner-operator dominance, channel fit, and outreach reachability. You get back a side-by-side scorecard so you can see how each niche stacks up.

### Why this matters
Three niches sound easy to compare by gut, but operational fit is hard to eyeball. One niche might have a great ticket but be saturated by agencies. Another might be wide-open but have an AOV too low to justify the retainer. The 8-dimension scoring surfaces these trade-offs explicitly so you're not picking blind.

### Goal
Give you objective operational fit data across 8 dimensions for each niche, so you can choose with eyes open. **This is a guide, not a gate.** All three niches can work — the assessment helps you understand where each one will need extra attention.

### Inputs
- 3 candidate niches from the member, each as a name (and optionally context/notes)

### How to run
1. Open a fresh AI session with the [niche-validation-prompt.md](niche-validation-prompt.md) loaded.
2. Provide the 3 niches.
3. The AI returns:
   - STRONG/MODERATE/WEAKER scoring across 8 dimensions per niche (ticket economics, market size, agency saturation, demand problem, B2C alignment, owner-operator dominance, outbound data, SMS/phone reachability)
   - Fit summary per niche (Strong Fit / Workable Fit / Requires More Effort)
   - Structured fields: AOV, ROI math, geographic concentration, Facebook Ads fit, fit score 1–10
   - Ranking 1–3
   - A closing line reminding the member that all three niches can work — the assessment helps them see operational fit, not a verdict

### Output
A validation summary covering all 3 niches with a clear winner.

### Decision logic
- **Clear top choice (3+ point fit-score gap):** Stage 2 on that niche only is usually enough. The member can still ask for a deep dive on a runner-up if they're curious.
- **Close call (top 2 within 1–2 points OR member wants to compare):** carry both into Stage 2 as comparison candidates. The member commits after reading both.
- **All 3 score "REQUIRES MORE EFFORT":** the niches can still work — just plan for more execution overhead. Walk the member through which dimensions are weaker and what that means operationally. The member chooses based on personal fit, not solely on the assessment scores.

---

## Stage 2 — Deep Dive(s)

### What's happening
Claude Code spawns a research subagent (`market-researcher`) and asks it to produce a full business-model assessment for the niche you're considering. The agent uses web search to pull real industry data — market size, average tickets, who the national franchises and PE rollups are, what Facebook Ad CPLs look like, what creative angles win — and writes it up as a 7-section markdown document. If you're undecided between your top two niches, Claude Code runs the agent twice (one per niche) so you can compare apples-to-apples.

### Why this matters
The Stage 1 assessment tells you the score; the deep dive tells you the *why* behind the score. If a niche scored "Strong Fit" on Ticket Economics, the deep dive shows you the AOV table, source-cited. If a niche flagged "Moderate" on Owner-Operator Dominance, the deep dive names the active PE rollups and how fast the indie pool is shrinking. You're deciding based on evidence, not vibes.

### Goal
Generate enough sourced industry intel that you can commit to a niche confidently — and enough material for the campaign team to design a real build (creative angles, audience targeting, offer architecture) once you commit.

**Output is markdown only.** No `.docx` is produced at this stage. Multiple deep dives may be produced in parallel for comparison — they're working artifacts, not deliverables.

### Inputs
- 1 winning niche from Stage 1, OR top 2 if Stage 1 was a close call
- The Stage 1 validation rationale (carries forward into the bible later)

### How to run

Spawn the `market-researcher` subagent with this prompt template:

> You are doing a deep-dive business model assessment for the niche: **[NICHE NAME]**.
>
> Context: the member runs a lead-generation service for SMB local-service operators. Two channels matter and should be kept distinct:
> - **Outreach to acquire operators as clients = SMS** (cold pitch sent to operator cell phones). This is how the member finds and signs new clients.
> - **Offer sold to those clients = Facebook Ads management** — creative, targeting, funnel build, ongoing optimization. Voice AI and customer SMS support the operator on the back-end (missed-call recovery, qualification) as part of the delivered stack but aren't the headline pitch.
>
> Engagement is ~$3K upfront + $997/month, so the niche needs AOV high enough that 1 incremental booked job/month covers that fee.
>
> Produce 7 sections (~1.5 pages):
>
> 1. Industry snapshot — US market size, # of US businesses, YoY growth, market structure, top 5 national franchises / PE-backed brands
> 2. Service tiers and AOV — single-job pricing, full-engagement pricing, premium tiers, weighted average ticket
> 3. Solo operator economics — monthly job volume, gross revenue, gross margin %, net margin, owner take-home
> 4. Lead acquisition channels — ranked by primacy with CPL benchmarks (Google LSA, Google Ads, **Facebook Ads (primary focus — go deepest here)**, Angi/Thumbtack, referrals); identify where leads die
> 5. Owner-operator vs. consolidation — % independent vs. franchise vs. PE-rollup; name active 2025–2026 rollups
> 6. Operator profile and outreach reachability — typical owner age, tech sophistication, marketing maturity, willingness to invest in paid ads, and how reachable they are by SMS for outbound pitching (this matters because SMS is the channel used to acquire these operators as clients)
> 7. **Facebook Ads channel fit — primary section** — does FB work for this niche? CPL benchmarks. Top 3 winning creative angles. Recommended offer architecture (front-end loss leader, core ticket, on-site upsell, recurring play). Audience targeting parameters.
>
> Conclude with: a fit summary (Strong / Workable / Requires More Effort) and 2–3 areas the member should plan for if choosing this niche (saturation, regulatory, seasonality, etc.) — frame as planning notes, not warnings.
>
> Output: markdown with tables and inline source links. Be direct and evidence-based. Cite sources for any numerical claims.

If `market-researcher` isn't available as a subagent_type (fresh install requires a session restart), fall back to `general-purpose` with the same prompt.

### Output
A markdown deep dive saved as `[niche]_deep_dive.md` (one per niche if running comparisons).

### Things to flag for the member (not gates)
While reading the deep dive, surface anything in these areas that the member should know about as they decide:
- AOV vs. the $1K rough floor (lower AOV niches can still work but require higher booked-job volume)
- Operator pool size (smaller pools mean tighter geographic targeting)
- Consolidation pressure (PE rollups in the metro reduce the addressable indie pool)
- Facebook Ads channel maturity (visual product, geo-bounded, demographic on platform)

These are planning notes, not pass/fail gates. The member uses them to size up effort and adjust expectations — not to disqualify a niche they're drawn to.

---

## Decision Point — Member Commits to One Niche

### What's happening
Claude Code stops and asks you which niche you're committing to. You read the Stage 1 assessment + Stage 2 deep dive(s), weigh the operational data against your personal factors (interest, geography, experience, gut feel), and pick one.

### Why this matters
The Niche Bible is the final crystallization of *one* niche — once committed, it gets a polished Word document. We don't compile that document until you've actually picked, because polishing a deliverable for a niche you might not pursue is wasted work. This pause is intentional.

**Before Stage 3 begins, the member chooses one niche to commit to.** Present the Stage 1 assessment alongside the Stage 2 deep dive(s) and frame the decision openly:

> "Here's the assessment data and the deep dive(s). All the niches you submitted can work — the data above shows where each one's operational fit is strongest and where it'll need extra attention. Which niche feels right for you to commit to? Stage 3 and Stage 4 will run on the one you pick."

The member is free to choose any of the niches — including a runner-up or a niche that scored "Requires More Effort." Their personal interest, geographic match, and existing experience are valid factors alongside the operational data.

If the member is still undecided, options:
- Run an additional deep dive on a runner-up
- Pull more comparison data (CPL benchmarks, geographic concentration overlap, seasonality)
- Let them sit with it and come back

Producing a `.docx` for a non-committed niche wastes effort and pollutes the deliverable. Wait for the member to decide.

---

## Stage 3 — Offer Bank from FB Ad Library

### What's happening
Claude Code hands you off to a 5-minute task in the Meta Ad Library. You open Meta's free public database of all active ads, search your committed niche, scroll, and **screenshot 5–10 ads with clear offers** (a specific dollar amount, a specific deal, a clear hook). You drop the screenshots in the chat. Claude Code reads them, extracts the offers, and builds out a full 15-angle offer bank by combining what you captured with proven industry-standard angles for the niche.

### Why this matters
This step does double duty. **First, it confirms the channel is alive** — if real businesses are running Facebook ads in your niche for 3+ months, that's hard evidence that the channel works for that niche. **Second, it gives you a starting menu of offers** — the headlines, hooks, and dollar amounts you'll use to write your first batch of ads instead of starting from a blank page.

### Goal
Confirm the niche is economically alive on Facebook (real businesses spending real money for 3+ months) AND build the offer bank — your starting playbook for ad creative.

### Inputs
- Winning niche name
- Member's time: ~5 minutes

### How to run
Hand the member [phase_2_fb_ad_library.md](phase_2_fb_ad_library.md). Steps:

1. Member opens the Meta Ad Library, sets country = US / status = Active.
2. Searches the niche term, scrolls, screenshots **5–10 ads with clear offers**.
3. Sends the screenshots back.
4. AI extracts offers, layers in industry-standard angles, builds a 15-angle offer bank.

The bank is structured into 4 tiers:
- **Tier 1:** Front-end loss leaders (3 offers — tune-up, repair discount, replacement discount)
- **Tier 2:** Trust and transparency angles (3 offers — free quote, ROI framing, anti-sales)
- **Tier 3:** Hook angles (2 angles — founder voice, pain-aware diagnostic)
- **Tier 4:** Scale-stage industry-standard angles (7 angles — storm, emergency, financing, membership, pre-sale, etc.)

### Voice rule
The offer bank uses descriptive voice, not instructional. "Best for:" not "Use when:". No "How to use this bank" sections. The member submits the bible to Sam's team for execution; they do not act on it themselves. (See memory rule on member deliverable voice.)

### Output
A markdown offer bank saved as `[niche]_offer_bank.md`.

### Channel fit signal
If the Ad Library search returns no active ads, or only 1–2 advertisers running fresh ads with nothing 60+ days old, that's a meaningful signal that **Facebook Ads as the primary channel may not be the fastest path** for this niche. Surface this to the member before compiling the bible — they may want to reconsider, scope down (e.g., a different geography), or proceed knowing they'll be doing more demand-creation work since the channel is unproven there.

---

## Stage 4 — Compile the Niche Bible

### What's happening
Claude Code takes the three artifacts you've built — the Stage 1 assessment summary (showing how you arrived at this niche), the Stage 2 deep dive (the business-model intel), and the Stage 3 offer bank (the ad menu) — and stitches them into a single master markdown file using a template. Then it runs a Python script that converts the markdown into a polished `.docx` Word document with proper headings, tables, blockquotes, and formatting.

### Why this matters
This is the single document you'll keep, share, print, and reference going forward. Everything you need to know about your committed niche — why you chose it, what the business model looks like, what offers to test first — lives in one place. Open it in Word, Google Docs, or Pages. Print it. Hand it to anyone helping you build the campaign. It's the canonical reference.

### Goal
Stitch the three outputs into one polished `.docx` deliverable — your Niche Bible.

> **Stage 4 is gated on:** (1) the member having committed to a single niche, and (2) Stage 3 (offer bank) being complete for that niche. If either is missing, do not run this stage. The `.docx` is the final deliverable and shouldn't be produced for a niche the member hasn't committed to.

### Inputs
- Stage 1 validation summary (winner + rationale)
- Stage 2 deep dive markdown for the **committed** niche
- Stage 3 offer bank markdown for the **committed** niche

### Steps

1. Copy [niche_bible_template.md](niche_bible_template.md) to `[niche]_niche_bible.md` (e.g., `cp niche_bible_template.md hvac_niche_bible.md`).

2. Open the new file and paste each Stage's output into the matching slot:
   - **How This Niche Was Chosen** ← Stage 1 ranking + commitment rationale
   - **Section 1 — Business Model Deep Dive** ← Stage 2 deep dive (full 7 sections, fit summary, planning notes)
   - **Section 2 — Offer Bank** ← Stage 3 offer bank (4 tiers, common combos, pitfalls)
   - **Sources** ← merged source links from Stages 2 and 3

3. Run the converter:

```bash
# Run from inside the Niche Validation and Bible Build folder
python3 tools/build_offer_bank_docx.py [niche]_niche_bible.md
```

The script handles H1/H2/H3 hierarchy, blockquotes with grey left-borders, attribution lines, header-shaded tables, horizontal rules, and inline bold/italic/code automatically. Output saved next to the input as `.docx`.

### Output
A single Niche Bible `.docx` (~40–60KB) ready to hand to the member.

---

## What the Member Receives

A polished Word document with three logical sections:

1. **How This Niche Was Chosen** — small comparison table, commitment rationale. The member sees the rigor that led to the choice.
2. **Business Model Deep Dive** — industry size, AOV, unit economics, lead channels, consolidation map, Facebook Ads fit, planning notes.
3. **Offer Bank** — 15 ready-to-use ad angles in 4 tiers with examples and common combos.

That doc is the input to every subsequent stage — campaign build, lander build, outbound list build, ad creative, SMS sequences.

---

## Common Questions

**"What if I'm excited about a niche but it scored low?"**
Pick it. The assessment is a guide, not a gate — operational fit data isn't the only valid input. Personal interest and existing experience matter. The assessment just tells you where this niche will need extra effort, so you go in eyes-open.

**"What if my three niches all score 'Requires More Effort'?"**
That's still fine. It means none have an obvious operational tailwind, but all three can work — just plan for more execution overhead. You'd choose based on personal fit, geography, and which one's planning notes feel most workable to you.

**"Why do I have to screenshot ads myself? Can't AI just do it?"**
Meta blocks automated scraping of the Ad Library, so AI can't fetch the live ads. Doing it manually takes 5 minutes and gives you 10× the data quality of a third-party summary — you're seeing what's actually running today, not what was archived months ago.

**"What if I see no ads in the Ad Library for my niche?"**
That's a signal that Facebook Ads as the primary channel may not be the fastest path for this niche. It doesn't kill the niche — but it does mean you'd be doing more demand-creation work than usual. Surface this to the team before compiling, in case you want to scope geographically or pick differently.

**"Can I run this workflow on more than 3 niches at once?"**
Stick with 3. Three is the right number for forced trade-offs. More than that, the assessment becomes mush; fewer, and you risk skipping the comparison rigor that helps you pick well.

**"What happens after I get the Niche Bible?"**
That document is the input to every downstream stage — campaign build, landing page build, outbound list pull, ad creative writing, SMS sequence drafting. You hand it to whoever's helping you build.

---

## Common Mistakes (for the operator running the workflow)

- **Skipping Stage 1 because the member is excited about a specific niche.** Run the 8-dimension scoring anyway. Even if the member commits to that niche, the assessment data makes the bible's planning section richer and orients the campaign team.
- **Letting the member do market research instead of just screenshotting in Stage 3.** They submit; AI researches.
- **Using instructional voice in the bible** ("you should", "use this when") — violates the member-deliverable voice rule (see [CLAUDE.md](CLAUDE.md)).
- **Compiling the .docx for an uncommitted niche.** Wait for the explicit commit before Stage 4.
- **Combining adjacent sub-niches** (e.g., "garage door + window" or "HVAC + plumbing"). Each sub-niche has different operators, AOV, and offer architecture — assess them separately if both are candidates.

---

## Reference Files

| File | Purpose |
|---|---|
| [niche-validation-prompt.md](niche-validation-prompt.md) | The AI prompt that runs Stage 1 |
| [market-researcher.md](market-researcher.md) | Subagent definition for Stage 2 (install once to `~/.claude/agents/`) |
| [phase_2_fb_ad_library.md](phase_2_fb_ad_library.md) | Member-facing instructions for Stage 3 (Ad Library scrape) |
| [niche_bible_template.md](niche_bible_template.md) | Template to fill in for Stage 4 |
| [tools/build_offer_bank_docx.py](tools/build_offer_bank_docx.py) | Markdown→docx converter for Stage 4 |
| [tools/README.md](tools/README.md) | Tool usage notes |
