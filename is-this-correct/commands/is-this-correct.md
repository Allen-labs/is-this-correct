---
description: Verify a conclusion using falsifiability-driven analysis. Pass a claim as argument. The agent will run the four-step loop (conjecture → confirm/corroborate → falsify → adjudicate) and report whether the claim survives.
---

# Is This Correct?

Run the `is-this-correct` skill on the user's claim or question.

## What to do

Take the argument (the claim or question the user passed) and apply the four-step falsifiability loop from `SKILL.md`:

1. **Conjecture** — Restate the claim as a hypothesis; identify its theoretical basis.
2. **Confirm + Corroborate** — Check self-consistency, then find *independent* sources (official docs, cross-scenario behavior, other people's results).
3. **Falsify** — Specify how the hypothesis could be disproven, then actually try to disprove it (plug in a different scenario, check measured data, run the code, query a tool).
4. **Adjudicate** — Assign confidence: High / Medium / Low / Open.

## Output format

```
## Verification: <the claim>

### Hypothesis
  [Hypothesis] <restated claim>
  [Basis] <reasoning / source>

### Confirm + Corroborate
  [Confirm] <self-consistency check>
  [Corroborate] <independent sources>

### Falsify
  [Falsify method] <how to disprove>
  [Falsify execution] <what was checked>
  [Falsify result] SURVIVED / FALSIFIED

### Adjudicate
  [Confidence] High / Medium / Low / Open
  <what remains unverified>
```

## Rules

- Never present an unfalsified hypothesis as a settled conclusion.
- "Survived" ≠ "proven correct" — it means "not yet overturned."
- If constructing a new metric/formula with no external basis, flag it explicitly.
- If falsified 3 times in a row, stop and tell the user the framework is likely wrong.
- If the question cannot be falsified, mark it **Open** and tell the user explicitly.
