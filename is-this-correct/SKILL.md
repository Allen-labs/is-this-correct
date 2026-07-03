---
name: is-this-correct
description: >
  Enforce falsifiability-driven verification for conclusions where the user cannot easily
  judge correctness themselves (unfamiliar domain, complex causation, no ground truth at hand).
  Use when you are about to state a conclusion that sounds right but could be wrong—and the
  cost of being wrong is real (wrong fix, wrong design, misleading the user).
  Triggers on: "is this correct", "is this true", "are you sure", "verify this claim",
  "double-check this", "could this be wrong", or when explicitly invoked via /itc.
  Do NOT use for: reading code to explain how it works, looking up API usage, debugging
  with visible error messages, or any task where the user can directly verify the answer.
---

# Is This Correct

You generate plausible hypotheses but cannot reliably distinguish correct from
plausible-but-wrong—when you confabulate, you genuinely believe you are right. This skill
forces you to **try to prove yourself wrong before presenting a conclusion**, letting
reality (data, code, docs, tools) be the judge.

> Self-consistency is a minimum gate, not proof. A conclusion is trustworthy because it
> **survived attempts to overthrow it**. One failed falsification eliminates a hypothesis;
> a hundred confirmations only mean it hasn't been overturned *yet*.

## The four-step loop

Run for **every non-trivial conclusion**. Do not present a conclusion as settled without
all four steps.

**Step 1 — Conjecture:** Formulate your best hypothesis. State as hypothesis, not fact.
Include theoretical basis and cite sources.

**Step 2 — Confirm + Corroborate:**
- *Confirm*: Does it produce a coherent, testable prediction? If not → back to Step 1.
- *Corroborate*: Find *independent* voices (official docs, cross-scenario, others' results).
  "I calculated it and it matches" = self-corroboration (weak). "Official spec says this too"
  = independent (strong). No corroboration → flag as weak.

**Step 3 — Falsify (core):** Try to prove yourself wrong.
1. *Method*: "If wrong, here's how we'd discover that: ..."
2. *Execute*: Plug in a different scenario, check measured data, run the code, query a tool.
3. *Result*: SURVIVED or FALSIFIED. If FALSIFIED → discard, return to Step 1. Do not quietly
   patch and pretend it was right.

**Step 4 — Adjudicate:**

| Condition | Confidence |
|-----------|-----------|
| Consistent + corroborated + survived | **High** (not yet overturned) |
| Consistent + survived, no corroboration | **Medium** |
| Consistent only, not yet falsified | **Low** |
| Cannot provide a falsification method | **Open question** |

If Low or falsified → loop to Step 1. If Open → stop, tell the user: "This is an open
question. I cannot verify it."

## Rules

1. Never present an unfalsified hypothesis as a settled conclusion.
2. "Survived" ≠ "proven correct" — phrase as "not yet overturned."
3. **Confabulation alert**: Constructing a new metric/formula with no external basis? Stop
   and flag: "This is my own construction with no external basis."
4. **3-strike rule**: Falsified 3 times in a row → stop. Tell the user: "My framework is
   likely wrong, not just the latest hypothesis. Use tools for ground truth or question
   core assumptions."
5. Use tools (run code, query data, read source) when reasoning stalls.

## Output template

```
## Analysis: <topic>

### Hypothesis 1
  [Hypothesis] <claim>
  [Basis] <reasoning / source>
  [Confirm] <self-consistency: yes/no>
  [Corroborate] <independent sources: yes/no + detail>
  [Falsify method] <how to disprove>
  [Falsify execution] <what was checked>
  [Falsify result] SURVIVED / FALSIFIED + evidence
  [Confidence] High / Medium / Low / Open

### [Hypothesis 2 ...] (if previous was falsified)

### Conclusion
  <surviving hypothesis + confidence>
  <what remains unverified>
```

## Reference

For methodology background (Popper, case studies, failure modes), see
[references/methodology.md](references/methodology.md).
