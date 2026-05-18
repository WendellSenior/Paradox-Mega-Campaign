# Paradox Mega-Campaign — Project Context

This file is the handoff context for any AI assistant working on this project. It captures the project, the user, the design decisions made so far, the open questions, and the conventions for working here. Read it in full before doing any work.

---

## Project Overview

A multi-year multiplayer project chaining five Paradox Interactive grand strategy games into one persistent world via converters:

1. **Imperator: Rome** (with the Invictus mod) — 304 BCE start
2. **Crusader Kings III**
3. **Europa Universalis IV**
4. **Victoria 3**
5. **Hearts of Iron IV** — through 1948

Up to ~30 players. Weekly 3-4 hour sessions. Each subcampaign runs several months of real time with breaks between games. Total real-time horizon: roughly 2-3 years.

The final deliverable set is:
- A scoped, documented campaign design (in progress; v0.1 exists)
- A singleplayer dry run validating the entire converter chain end-to-end
- A project repository housing the converter chain playbook, saves, lore, admin tooling
- A GitHub Pages campaign website (player-facing, between-session worldbuilding home)
- Lightweight campaign administration tooling

The campaign itself begins after the dry run and the supporting infrastructure are in place.

---

## About Cosmo (the user)

- **Role**: Sole organizer AND a participating player. Wears both hats.
- **Tech background**: Mostly non-technical. Wants the AI assistant to lead on architecture, code, repo design, website, and admin tooling. Cosmo reviews, directs, and provides content.
- **Working style**: Thoughtful. Asks for tradeoff analysis rather than pre-decided answers. Open to staged planning. Will tell you when they want to discuss vs. when they want you to just build something.
- **Player group**: Already exists — ~30 interested people lined up. This is not a "find players" problem; it is a "design a system that 30 real people will sign up for and stay engaged with over 2-3 years" problem.

---

## Design Decisions (Locked)

All of these were settled during scoping conversations on 2026-05-17. They are written up in detail in the design document (see "Key Artifacts" below). The short version:

### Roster model: Credits-Based Capped Persistent
- All ~30 players have **persistent identity** across all five games — no rotating per-game wipe.
- Session attendance is rate-limited by a **credits** system. Every player gets enough baseline credits to play ~5 sessions per subcampaign minimum. Excellent RP and good behavior earn bonus credits. When out of credits, players sit out until credits regenerate.
- A **reserve list** holds players waiting to enter, and is load-bearing (see below).

### Reserve list: load-bearing pool, four jobs
1. Hold new players who have joined but not yet been placed.
2. Provide substitutes for absent regulars (see "Absent player handling" below).
3. Fill newly-scoped nations at each geographic expansion (see below).
4. Receive fragments produced by mandatory succession-crisis events at converter transitions (see "Bloat handling" below).

Implication: reserve players cannot be passively benched. They need onboarding sufficient to substitute on short notice, and an explicit cycling expectation so reserve feels like a path to active play, not a holding pen.

### Geographic scope: expanding by era
| Game | Playable region |
|------|-----------------|
| Imperator (Invictus) | Italy to Persia |
| Crusader Kings III | England to Pakistan |
| Europa Universalis IV | England to Russia to India |
| Victoria 3 | Whole world |
| Hearts of Iron IV | Whole world |

New regions opening at each transition (Spain at EU4 start, Americas mid-EU4, Africa/East Asia at Vic 3) go to **reserve-list players at reduced credit cost**.

### Version pinning
Each game is locked to the patch where its corresponding converter is most stable, even if newer patches exist. Stability over recency. Specific pins are finalized during the singleplayer dry run.

### Mod policy
Only mods explicitly designed for converter compatibility. Invictus is in by default. Everything else is presumptively out unless it has been validated against a full conversion pass.

### Bloat handling: structural + voluntary, no mods, no overt penalties
- **Mechanism 1**: Mandatory **succession-crisis events at converter transitions** for nations exceeding a defined size threshold. Original player keeps the core (e.g., England-proper after a CK3 united Britain); periphery fragments to reserve players or AI. Framed as **historical inevitability**, not punishment.
- **Mechanism 2**: Credits-driven **voluntary pivots**. Players who have "won" their subcampaign can retire their nation at conversion in exchange for bonus credits and/or first-pick rights on a new nation in the next game.
- **Explicitly rejected**: mods (break converter chain) and overt admin penalty debuffs (anti-fun).

### Absent-player handling: substitute as Regent
- When a credited regular can't attend, a reserve player substitutes for the session.
- Substitutes play as a **named regent character** with limited but real agency: may continue existing wars, negotiate truces, execute pre-stated standing orders, handle administration. May NOT declare new wars, form/break alliances, change religion or government, or partition the realm.
- Regent characters **persist in the world** — they can become the seed for a player's own nation if they later receive a fragment from a succession crisis.

---

## Open Questions (Not Pre-Decided)

These are deliberately unsettled and belong in front of the playerbase. The design doc has organizer recommendations attached as starting positions for discussion, but no answer has been adopted yet.

- Credits regeneration rate.
- Bonus-credit award rubric (what specifically earns bonus credits, how is it kept fair).
- Credit reset behavior across subcampaigns (recommendation: full reset, no carryover).
- Credit cost for absent players whose nation is being substituted (recommendation: half credit).
- Reserve list maximum size.
- Substitute-appearance guarantee for reserves (e.g., one sub every N sessions).
- Reserve player participation during interregnum periods (AAR contributions, lore submissions).
- First-claim rights for existing players who deliberately migrate into not-yet-scoped regions in the prior game.
- Bloat threshold — what province/development count triggers mandatory fragmentation.
- Fragmentation decision authority (which fragments go where, who decides).
- Antagonist-role opt-in pathway for players who decline both fragmentation and voluntary pivot.
- Standing-orders submission format and deadline.
- Consecutive-absence policy and reserve-readmission rules.

---

## Where the Work Is Now

Tasks, in order of dependency:

1. **Scope the mega-campaign concept** — DONE. Design document v0.1 exists. Awaiting Cosmo's review and group discussion.
2. **Plan the singleplayer dry run** — NEXT. Blocked on review of (1). Goal: a written plan for a condensed end-to-end SP run that validates the converter pipeline (Imperator→CK3→EU4→Vic3→HoI4), produces a "converter chain playbook," and finalizes version pins.
3. **Design the repo + folder architecture** — pending. Likely runs in parallel with (2). The on-disk structure needs to grow over 2-3 years without sprawl. Houses converter binaries, save archives, the playbook, lore docs, admin tooling, and website source.
4. **Build the GitHub Pages campaign website** — pending. Blocked on (3). Player-facing site for nation pages, session AARs, maps, dynasty trees, timeline. Low-maintenance, static.
5. **Stand up campaign administration tooling** — pending. Blocked on (3). Lightweight processes/templates for tracking attendance, credits, treaties, claims, inter-session narrative threads across 30 players.
6. **Run the live campaign** — pending. Blocked on (2), (4), (5).

---

## Key Artifacts in This Folder

- **`Paradox Mega-Campaign - Design Document v0.1.docx`** — The primary scoping artifact. ~9 pages. Sections cover project pitch, game chain + version pinning, cadence, roster system, reserve list, geographic scope, bloat handling, absent-player mechanics, dry run plan, risks, interregnum content, consolidated open questions, and next steps. This is the document Cosmo will share with the playerbase for review.
- **`CLAUDE.md`** (this file) — handoff context for AI assistants.

Future artifacts (not yet created):
- Converter chain playbook (from the SP dry run)
- Repo architecture spec
- Website source
- Admin tooling templates

---

## How to Work in This Project

### General conventions

- **Lead on the technical side.** Cosmo is non-technical and explicitly wants the AI to make architectural and implementation decisions, not just suggest options. Present a recommendation as a recommendation, not as a list of equivalents.
- **But surface tradeoffs on design questions.** Cosmo's preferred mode for non-technical creative/policy questions (roster model, bloat handling, etc.) is to be shown the actual tradeoffs and choose with that context. Don't pre-decide what should be in front of the group.
- **Avoid heavy formatting in chat responses.** Prose over bullets for discussion. Headers and tables are appropriate in deliverables (the design doc, the playbook, the website) but not in conversational answers.
- **Honesty about risk.** This is a 2-3 year commitment. Pretending things are simpler than they are will get caught later. Flag attrition risk, MP stability issues at 30-player scale, converter pipeline rot, and burnout risk where they apply.
- **Be sustainable-by-default.** When designing systems (admin tools, repo structure, website maintenance), favor low-friction options. The organizer is one person managing a lot.

### Specific working norms

- **Decisions get written down.** When Cosmo settles a design question, capture it in the design doc and/or CLAUDE.md. The conversation history is not durable enough for a 2-3 year project.
- **Open questions stay open.** Do not let an organizer recommendation in the doc collapse into a settled decision without explicit playerbase input — the doc is meant to be shared, and pre-deciding things kills group buy-in.
- **The reserve list is load-bearing.** Any new mechanic that touches the playerbase needs to account for what it does to reserves. The four jobs the reserve does are not separable.
- **The converter chain is the spine.** Anything that threatens converter stability (mods, patch updates, untested mechanics) is presumptively rejected. The cost of converter stability is accepted in exchange for the spine of the campaign.

---

## Risk Flags (Persistent)

Three things that quietly kill mega-campaigns. Flag and plan around them at every stage.

1. **MP stability at scale.** Imperator and CK3 handle 30 players. EU4 becomes desync-prone but manageable. Vic 3 and HoI 4 are notorious for MP issues at scale. Likely mitigations: reduced active count in later games (via credits + natural pivots), regional sub-sessions, or accepting more reloads.
2. **Attrition.** Over 2-3 years, expect to lose 30-50% of any 30-person group to life events. The credits-plus-reserve model is the primary defense. Plan emotionally for this too — a player leaving isn't failure, it's the timeline.
3. **Converter pipeline rot.** Paradox patches games. Converter teams patch converters. Any can break the chain mid-campaign, especially for Vic 3 and HoI 4. Defense: strict version pinning, no auto-updates, no surprise mods, the SP dry run validates end-to-end before MP starts.

---

## Quick Reference: Acronyms and Terms

- **AAR** — After Action Report. Narrative writeup of a session or subcampaign.
- **Bloat** — when a player's nation becomes too large/dominant for the next game to be fun for them or fair to others.
- **Conversion / converter** — the process or tool that translates a save file from one game to the next in the chain.
- **Fragmentation event** — a mandatory succession crisis at conversion that breaks up an oversized nation.
- **Interregnum** — the months-long break between subcampaigns. Where narrative work happens and momentum is either preserved or lost.
- **Invictus** — the CK3 mod that includes the Imperator-to-CK3 converter and a richer ancient/medieval world.
- **Pivot** — a player voluntarily retiring their nation at conversion and picking a new one, in exchange for bonus credits or first-pick rights.
- **Regent** — the named character a reserve player plays as when substituting for an absent regular.
- **Reserve list** — the pool of players who substitute, receive fragments, and fill newly-scoped regions. Not a passive waiting room.
