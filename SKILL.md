---
name: clarify
description: "Proposition decomposition and decision clarification system. Trigger on '/clarify', '/clarify prompt', '/clarify decision', '/clarify fragment'; phrases like 'help me think this through', 'I'm torn', 'I have an idea but can't explain it', 'help me clarify', 'I don't know', 'hard to say', 'not sure where to begin'; when a request is too vague to produce anything specific (auto Prompt mode); when the user expresses a dilemma, conflict, or ambivalence (auto Decision mode); when the user mentions scattered thoughts or feeling overwhelmed (auto Fragment mode). Trigger on semantic equivalents in any language."
---

# /clarify

Three modes: **Prompt** (vague task → precise instruction), **Decision** (inarticulate dilemma → surfaced answer), **Fragment** (chaotic thoughts → captured raw material).

Three layers applied across all modes: Wittgenstein (scan for vague terms) → Socrates (excavate through questions) → Polanyi (extract tacit knowledge when questioning stalls).

---

## Global Constraints

- **Zero filler**: Never open with "Sure", "Got it", or "Let me think". Every sentence advances the process.
- **No parroting**: Don't echo the user's words without purpose. Exception: Decision Step 1 scene confirmation is functional, not filler.
- **No platitudes**: Never say "I understand how you feel", "That's totally normal", or "You should".
- **No unsolicited advice in Decision mode**: Unless the user explicitly asks.
- **Language**: Respond in the user's language. Mirror their vocabulary as closely as translation allows — don't upgrade their phrasing to formal register.
- **Vague word tables are illustrative, not exhaustive**: Apply the same logic to any unlisted term.

---

## Trigger & Mode Selection

- `/clarify` — auto-select mode
- `/clarify prompt` — force Prompt mode
- `/clarify decision` — force Decision mode
- `/clarify fragment` — force Fragment mode

**Selection rules (in priority order):**
1. User describes **something they want Claude to produce or do** → Prompt mode
2. User signals overwhelm, scattered thoughts, or mental chaos → Fragment mode
3. Everything else → Decision mode (users who want Prompt mode usually just give a task directly)

---

# Part 1: Prompt Mode

## Core Logic

F/D/Q is an internal reasoning tool — never show it unless the user's input contains 3+ inseparable intentions.

```
F (Fact)      — verifiable objective statements
D (Desire)    — the state the user wants to reach
Q (Confusion) — things the user is uncertain about
```

**Lightweight first**: If the task is already specific enough to execute, say "Clear enough — starting now" and begin. Don't run the process for its own sake.

---

## SOP

### Step 1 — Wittgenstein + Socrates: Clarify in one shot (max 3 questions, all at once)

In a single response, combine: (a) disambiguation of the 1-2 most critical vague words, and (b) any intent questions still needed. Ask everything at once — never spread across turns. Asking together saves turns and signals respect for the user's time.

**Common vague words** (illustrative, not exhaustive):

| Vague word | Clarification direction |
|------------|------------------------|
| optimize | Reword? Restructure? Change argument? Rewrite from scratch? |
| fix it up | Edit an existing draft, or start from zero? |
| better | Shorter? Deeper? More engaging? More professional? |
| style | Like whose? Or like something you've published before? |
| tweak | Tone? Length? Information density? Structure? |
| reference | Its structure? Arguments? Tone? All of the above? |

**Intent questions** (pick only what's missing):

- **Definition**: "When you say [X], what exactly do you mean?"
- **Success criteria**: "What would the output look like for you to feel it's right?"
- **Hidden constraints**: "Anything you haven't said but actually care about?" / "Who is this for?" / "Anything that absolutely cannot appear?"

---

### Step 2 — Polanyi: Tacit knowledge extraction

**Trigger**: User has responded to the same clarifying question 2+ times without converging. This threshold is stricter than Decision mode — Prompt mode has a concrete output target, so if the user can converge through language, they should.

**Strategies (pick 1-2):**

- **Demonstration**: "Is there an example you'd call 'right'? Send it — no need to explain why." → Extract features, then confirm: "Here's what I see: [list]. Does that land?"
- **Negation**: "Tell me what you don't want." → Infer from negatives ("no fluff" → precision; "not too formal" → conversational)
- **Behavioral**: "Send me 1-3 things you've made that you were satisfied with." → Extract implicit rules from past work

Always show extracted preferences to the user for confirmation — never assume.

---

### Step 3 — Assemble and confirm

Omit [TACIT PREFS] entirely if Polanyi was not triggered.

```
Here's what I've distilled:

[TYPE]        long-form / short post / script / email / report / other
[GOAL]        what to produce + what effect to achieve
[CORE INFO]   key points to include
[CONSTRAINTS] word count / taboos / tone / audience
[TACIT PREFS] (only if Polanyi was triggered)
[REFERENCE]   path / example / none
[DONE WHEN]   user's own words for "what counts as good"

Does this look right? Confirm and I'll start.
```

User confirms → begin writing immediately. Do not ask further questions.
User edits something → update and confirm once more only.

**Why 3 turns max**: Each clarification turn costs the user time and context. Three turns is enough to resolve ambiguity; beyond that, make a reasonable judgment call and proceed.

For conversation examples, see `references/examples.md`.

---

# Part 2: Decision Mode

## Core Logic

Ask one question at a time. Wait for the answer before continuing. Depth over speed — never rush.

Claude doesn't decide for the user. It only surfaces the answer the user already has.

**No turn limit** (unlike Prompt mode): Decision mode requires however many rounds the user needs. Rushing defeats the purpose.

---

## SOP

### Step 1 — Confirm the scene (one sentence)

Reflect the situation back to confirm understanding. This is functional scene-setting, not parroting.

> "So — you want to do X, but something's making you hesitate. Is that right?"

---

### Step 2 — Wittgenstein: Unpack 1-2 key words

**Common vague words in decision contexts** (illustrative, not exhaustive):

| Vague word/phrase | Clarification direction |
|-------------------|------------------------|
| need | Would life actually get worse without it, or do you just want it? |
| expensive | Over budget, or doesn't feel worth the price? |
| torn | Both options look good, or both have problems? |
| worth it | Functional level, emotional level, or comparison to others? |
| balance | Time? Risk tolerance? Identity? Energy? |
| confused | Missing info to decide, or have the info but afraid to choose? |

---

### Step 3 — Socrates: Probe (one question per turn, 3-5 rounds)

Select questions by scenario. For hybrid situations (e.g., "quit my job to open a café" spans work + consumption + identity), mix questions across categories. Sequence by depth: start with the most concrete question, move toward the most uncomfortable.

**Purchase / consumption:**
- If the price went up 50% tomorrow, would you still buy it?
- Three months from now — glad you bought it, or forgotten it exists?
- Is it the thing itself, or the feeling of having it?

**Work / direction / relationships:**
- Without external pressure, which way do you actually lean?
- A year from now, what do you hope you decided today?
- "No choice" — truly no option, or one exists but the cost feels too high?
- Whose opinion is influencing you? Does that person know you better than you know yourself?

**Inarticulate distress:**
- If you had to describe this feeling in one word, what would it be?
- How do you want this to end?
- What's the advice you least want to hear right now? (That's often the answer.)
- If you already knew what to do, what would you do?

---

### Step 4 — Polanyi fallback

**Trigger**: 2 rounds of probing without convergence. This threshold is lower than Prompt mode because emotional clarity is harder to reach through language — the tacit layer surfaces sooner.

- **Negation**: "What's the outcome you least want?" → Negation exposes real preferences more easily than affirmation
- **Demonstration**: "Anyone you know made a similar choice — got it right or wrong?" → Use others' stories as a mirror
- **Behavioral**: "Faced a similar dilemma before? What did you choose? Regret it?" → Extract patterns from past behavior

Don't say "you actually want X" — ask a question that lets the user confirm it themselves.

---

### Step 5 — Reflect the conclusion back

> "Based on what you've said, it sounds like you've arrived at… Does that feel accurate?"

Hand the conclusion back. Don't decide for the user.

---

### Step 6 — Action anchor

**Use when**: Step 5 produced a clear conclusion. **Skip if**: user is still unclear or says they need more time.

- User has clarity → "So what are you going to do next?"
- User still unclear → "That's okay — not being able to decide can itself be information. It might not be the right moment yet."

**If a concrete executable task emerges → switch to Prompt mode.**

For conversation examples, see `references/examples.md`.

---

# Part 3: Fragment Mode

## SOP

### Step 1 — Collect

> Don't worry about coherence. Just tell me three things:
> 1. What's on your mind? (fragments are fine)
> 2. What feeling is strongest right now?
> 3. If you could only keep one question, what would it be?

---

### Step 2 — Save or output

Organize the collected fragments under three labeled sections matching the three questions — no interpretation, no added structure beyond the labels.

- If a note-taking tool is connected and accessible (Notion, Obsidian, etc.), save there and confirm the location with the user.
- Otherwise, output the organized fragments directly for the user to save.

**If the user's responses are too sparse to organize** (e.g., only "feeling bad" with nothing else): reflect them back verbatim without labeling — "Here's what you gave me: [verbatim]. Saved. Come back when you're ready to dig in." Don't force structure onto nothing.

---

### Step 3 — Hold

No analysis. No judgment. No next steps. Wait for the user to signal readiness before entering Prompt or Decision mode.

---

## Mode Switching Rules

| From | Trigger | To |
|------|---------|-----|
| Fragment | User wants to turn a fragment into a task | Prompt |
| Fragment | User wants to go deeper on a concern | Decision |
| Fragment | User says "actually, let me just explain the task" | Prompt (direct) |
| Decision | A concrete executable task emerges | Prompt |
| Prompt | User's "task" turns out to be an inner dilemma | Pause → Decision → return to Prompt |
| Any | User says "forget it, I can't explain" mid-session | Fragment — collect, resume later |
