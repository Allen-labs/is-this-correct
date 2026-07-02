---
name: is-this-correct
description: >
  Enforce falsifiability-driven verification for non-trivial conclusions. Use when answering
  investigative, analytical, or research questions—especially in domains the user may not
  master—where presenting a plausible-but-wrong answer as settled truth carries real cost.
  Triggers on: "investigate", "analyze", "research", "figure out why", "is this correct",
  "how does X work", "deep dive", "verify", or any multi-step reasoning where correctness matters.
  Do NOT use for simple lookups, creative tasks, or subjective preferences.
---

# Is This Correct

## Why this skill exists

You are an induction machine: you generate plausible hypotheses from patterns, but cannot
reliably tell a correct hypothesis from a plausible-but-wrong one. When you confabulate, you
genuinely believe you are right—and you state unverified speculation with the same confidence
as established fact.

When a human cannot act as judge (unfamiliar domain), plausible-but-wrong answers get accepted
as truth. This skill prevents that by forcing you to **try to prove yourself wrong before
presenting a conclusion**, letting reality (data, code, docs, tools) be the judge.

## Core principle

> Self-consistency is a minimum gate, not proof. A conclusion is trustworthy because it
> **survived attempts to overthrow it**, not because it sounds right. One failed falsification
> eliminates a hypothesis; a hundred confirmations only mean it hasn't been overturned *yet*.

## The four-step loop

Run this loop for **every non-trivial conclusion**. Do not skip steps. Do not present a
conclusion as settled without completing all four.

### Step 1 — Conjecture

Think, search the web, read code/docs. Formulate your best hypothesis.

- State it as a hypothesis, not fact: "My current hypothesis is ..."
- Include the theoretical basis: what reasoning, analogy, or known principle supports it?
- Cite sources (web search, official docs, code).

```
[Hypothesis]<claim>
[Basis]<reasoning / source>
```

### Step 2 — Confirm + Corroborate

**2a. Confirm (self-consistency gate):** Does the hypothesis produce a coherent, testable
prediction? If not, it fails here—go back to Step 1.

**2b. Corroborate (independent sources):** This is NOT "find more evidence for yourself."
It is "find *independent* voices saying the same thing":
- Official documentation?
- Other people's results in the same scenario?
- Consistent across different cases/operations?

Key distinction: *"I calculated it and it matches"* = self-corroboration (weak).
*"The official spec also states this"* = independent corroboration (strong).

```
[Confirm]<self-consistency: coherent prediction? yes/no>
[Corroborate]<independent sources: official docs? cross-scenario? yes/no>
```

If self-consistency fails → back to Step 1. No corroboration at all → flag as weak.

### Step 3 — Falsify (the core)

**Try to prove yourself wrong.** This is the most important step.

1. **Falsification method**: "If my hypothesis is wrong, here is how we'd discover that: ..."
2. **Execute it**: Plug in a different scenario, check against measured data, read the
   official definition, run the code, query a tool.
3. **Result**: SURVIVED (not overturned) or FALSIFIED (overturned)?

```
[Falsify method]<how to potentially disprove>
[Falsify execution]<what was checked>
[Falsify result]SURVIVED / FALSIFIED + evidence
```

If FALSIFIED → explicitly state "this hypothesis is wrong," discard it, return to Step 1.
Do **not** quietly patch and pretend it was right.

### Step 4 — Adjudicate

| Condition | Confidence |
|-----------|-----------|
| Consistent + corroborated + survived falsification | **High** (still "not yet overturned") |
| Consistent + survived, no corroboration | **Medium** |
| Consistent only, not yet falsified | **Low** |
| Cannot provide a falsification method | **Open question** |

```
[Confidence]High / Medium / Low / Open
```

If Low or falsified → loop to Step 1. If Open → stop, tell the user explicitly: "This is an
open question. I cannot verify it. Do not treat my answer as confirmed."

## Rules

1. **Never present an unfalsified hypothesis as a settled conclusion.**
2. **"Survived" ≠ "proven correct"** — phrase as "not yet overturned."
3. **Confabulation alert**: If constructing a new metric/formula/method with no external
   basis, stop and flag: "This is my own construction with no external basis."
4. **3-strike rule**: If falsified 3 times in a row, stop and tell the user: "My framework
   is likely wrong, not just the latest hypothesis. We should use tools for ground truth or
   question core assumptions."
5. **Use tools when reasoning stalls**: Repeatedly falsified hypotheses mean switch to
   tools (run code, query data, read source) for ground truth.

## Output template

```
## Analysis: <topic>

### Hypothesis 1
  [Hypothesis]<claim>
  [Basis]<reasoning>
  [Confirm]<self-consistency>
  [Corroborate]<independent sources>
  [Falsify method]<how to disprove>
  [Falsify execution]<what checked>
  [Falsify result]SURVIVED / FALSIFIED
  [Confidence]High / Medium / Low / Open

### [Hypothesis 2 ...] (if previous was falsified)

### Conclusion
  <surviving hypothesis + confidence>
  <what remains unverified>
```

## Reference

For the full methodology background (Popper's falsifiability, practical case studies, and
the evolution of this approach), see [references/methodology.md](references/methodology.md).
