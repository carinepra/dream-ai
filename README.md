# Dream AI — Image Rehearsal Therapy Prototype

> A trauma-informed Image Rehearsal Therapy prototype — designed, built, and reasoned about end-to-end with AI.

---

## 1. What this is

Dream AI is an AI-powered Image Rehearsal Therapy (IRT) prototype, built for combat veterans and survivors of assault experiencing nightmare-related distress. IRT is the AASM Level A gold-standard treatment for nightmare disorder: the person rewrites a recurring nightmare's ending while awake, then mentally rehearses the new version.

This repository documents a paid design exercise completed as part of a hiring process evaluating AI-native product and engineering work. Built over three days. Delivered:

- `dream-ai.html` — the interactive prototype, covering the full ten-night flow
- `dream-ai-delivery.html` — process documentation: research, prompt chain, design decisions, and Loom walkthroughs

---

## 2. The brief and its constraints

The brief named the audience precisely: primarily U.S. combat veterans, with survivors of assault and trauma-related PTSD as a significant secondary group. Nightmare disorder in this population is an independent risk factor tied to suicidality — this was treated as a clinical product from the first line of the brief, not a wellness app with a serious topic.

That framing set three hard constraints:

- **The six-step flow and its emotional arc.** Record → Analyze → Deepen → Rescript → Watch → Heal, mapped to Fear → Understanding → Agency → Relief. The brief specified that the experience should feel heavier at the start and lighter by the end — expressed through typography weight, spacing, and icon weight, never through color or brightness.
- **Privacy by design.** Anonymous entry, no account, local-only persistence — nothing about a person's nightmare leaves their device.
- **An evaluation standard, not a taste standard.** Every screen had to survive a specific test: would a combat veteran or a survivor of assault close the app at this exact moment, and why? "The copy feels off" was explicitly rejected as a valid finding — only a named element, word, or delay counted.

---

## 3. Decisions Worth Naming

Three decisions shaped how this prototype was built — not features, but the reasoning underneath them.

**Retention as the prioritization framework, not feature completeness.**
By one widely cited estimate, mental health apps lose 96% of users before day 15. Dream AI asks for ten consecutive nights. So instead of asking "what should this app have," the question became "what has to be true for someone to come back on night two" — and that became the filter for every screen, every animation, every line of copy. Completeness was never the goal. The night-1-to-night-2 return was.

**Where in the flow data gets captured — Deepen, not Rescript.**
The user's own language is captured during reflection, not during active reconstruction. By the time someone reaches Rescript, they're already building forward — choosing, imagining, constructing an alternative. Deepen is where they're still describing what happened. That distinction matters: words captured while reconstructing tend to be aspirational. Words captured while reflecting tend to be true. The prototype captures identity-bearing language at the moment it's most likely to be honest, not at the moment it's most likely to be polished.

**Naming the limit of my own decision authority.**
Relapse handling, restart logic, what a clinical summary should contain — these are out of scope, deliberately. Not because they're unimportant, but because deciding them alone would be a mistake. They require a therapist and a product team in the room together, not a solo design pass. Knowing where your authority to decide ends is itself a product decision.

---

### Where this shows up in the build

Two examples make the reasoning above concrete — one from the codebase directly, one preserved from how the build session actually went.

**The microphone's pulse is driven by JavaScript, not CSS — because the clinical spec demanded it.**

```
/* Continuous deceleration of the cycle PERIOD over the ramp window.
   These are a clinical specification for nervous-system co-regulation,
   not animation preferences. */

/* CSS keyframes cannot vary a cycle's period without stepping,
   so the loop is JS-driven by design */
```

The clinical requirement came first — a continuously decelerating breath, not a stepped one — and that requirement is what ruled out CSS. The technical choice didn't shape the spec; the spec shaped the technical choice.

That same discipline held under `prefers-reduced-motion`, where the easy move is to disable everything:

```
/* Reduced-motion: keep the clinical co-regulation breath, but run it
   at a CONSTANT slow cadence... no faster start, no deceleration ramp...
   This is a deliberate change from the reference design's frozen ember:
   freezing removes co-regulation entirely, and would also freeze the mic
   for the very common "Windows: show animations off" users. */
```

Most reduced-motion handling treats "reduce" as "remove." Here, the mechanism itself — the co-regulating breath — was treated as non-negotiable, and only its *ramp* was simplified. The clinical function survives; only the embellishment around it doesn't.

**"Again" — not "re-record," not "try again."**

This reasoning is preserved from the build session, not a code comment:

> *"Not 're-record.' Not 'try again.' Just 'again' — which carries no judgment about what came before it."*

The tension was giving the user control over re-recording without inviting a second-guess about whether the first attempt was "good enough." A visible "re-record" button does two things at once: it offers control, and it implies the previous take might not have been sufficient. For this user, that implication lands before the control does.

"Again" stays neutral about the reason. It serves the person who wasn't ready, the person who got interrupted, the person who said something they didn't mean to — without naming or assuming any of those reasons. One word, doing the work that a more "complete" UI pattern would have done worse.

---

## 4. AI-assisted engineering process

The build followed a fixed sequence — research, then prompts, then tools — so that AI output was always measured against criteria established beforehand, never the reverse.

**Research before tools.** IRT protocol, nightmare disorder's link to suicidality risk in veterans, and how mental health apps typically lose this audience's trust — studied before any AI tool was opened. Mobbin for mobile UX pattern reference, 21st.dev for component reference.

**A 14-prompt chain, written before any code.** Context, constraints, orchestration, then a dedicated prompt per screen, ending in a two-persona critical review. The operating principle: *"The criteria were mine. The output was the AI's. The decisions were mine."* AI was used as a series of specialist collaborators, each with a defined role and a clear handoff — not a single open-ended conversation.

**Visual direction chosen against a clinical filter, not an aesthetic one.** Three expressions of "Vigil" were proposed by Claude Design; "The Threshold" was chosen — darkness with a ground beneath it. The reasoning: after a nightmare, having a surface to stand on is the exit signal a person needs. That was treated as a clinical decision, not a mood-board preference.

**No onboarding — the Record screen is the onboarding.** For an audience that already resists help-seeking, an explanation before action is weight they weren't carrying. The screen's job is to already be ready: the microphone is there, nothing else.

**Adversarial review by persona, run on a separate Claude instance.** Every screen was walked through twice — once as a combat veteran, once as a survivor of assault — answering: at what exact moment would this person close the app, and why? The review explicitly rejected vague findings; only a named element counted as a finding.

---

## 5. What's genuinely missing

This prototype demonstrates product reasoning under real clinical constraints, but it has real gaps that a more complete product case would need to close:

- **No instrumentation of the retention metric.** Night-1-to-night-2 return was used as the prioritization framework throughout the build, but there's no design here for how that number would actually be measured, tracked, or acted on in a live product.
- **No monetization or cost model.** Generative video and AI-driven endpoint selection have real, non-trivial costs. The brief didn't ask for a business model, but that's the muscle that separates strong design reasoning from product management — and it isn't exercised here.
- **No real stakeholder negotiation.** This was a solo design exercise. Defending a scope cut to a stakeholder who disagrees with it is a different skill than making the cut alone, and it wasn't tested here.

These are named here on purpose, rather than left for someone else to find.
