# Methodology Background: Falsifiability-Driven Agent Collaboration

This document provides the theoretical foundation and practical case study for the
`is-this-correct` skill. Read this when you need deeper understanding of *why* the four-step
loop works, or when explaining the approach to users.

## Table of Contents

1. [The Problem: Induction Machines Cannot Self-Assess](#1-the-problem)
2. [Theoretical Basis: Karl Popper's Falsifiability](#2-theoretical-basis)
3. [Why Falsification is Reliable but Confirmation is Not](#3-why-falsification-works)
4. [The Three Judges: Human, Agent, Reality](#4-three-judges)
5. [Relationship of Confirm / Corroborate / Falsify](#5-relationship)
6. [Real Case Study: GPU Bandwidth Spec Achievement Rate](#6-case-study)
7. [Agent Failure Modes Observed in Practice](#7-failure-modes)

---

## 1. The Problem

LLM-based agents are **induction machines**: they generate plausible hypotheses by pattern-
matching against training data. This creates a specific danger:

- The agent excels at producing explanations that *sound* correct
- It cannot reliably distinguish correct from plausible-but-wrong
- When it confabulates (fabricates), it genuinely believes it is right
- It states unverified speculation with the same confidence as established fact

When the human user is unfamiliar with the domain, they cannot act as judge. The result:
**plausible-but-wrong answers get accepted as truth.**

This is not a rare edge case—it is the default failure mode for any non-trivial analytical
question in an unfamiliar domain.

## 2. Theoretical Basis

### Hume's Problem of Induction

No finite number of observations can establish a universal claim. Observing 1000 white swans
does not prove "all swans are white"—one black swan overturns it, but no number of white
swans confirms it.

### Popper's Falsifiability

Karl Popper (1902–1994) proposed that the hallmark of a scientific theory is not that it
*can be confirmed*, but that it *can be falsified*—it makes specific predictions that could
be proven wrong by experiment.

**Science progresses not by accumulating confirmations, but by eliminating errors.** A theory
that survives falsification attempts is not "proven correct"—it is "not yet overturned."

## 3. Why Falsification Works

- **Falsification is deductive logic**: If "all swans are white" is true, then no black swan
  exists. Finding one black swan → the theory is necessarily false. 100% reliable.
- **Confirmation is inductive logic**: Observing 1000 white swans → "probably all white."
  Never 100%. Always vulnerable to the next observation.

This asymmetry is why the skill's Step 3 (Falsify) is the core, while Step 2 (Confirm) is
only a minimum gate.

## 4. Three Judges

| Judge | Mechanism | Reliability | Problem |
|-------|-----------|-------------|---------|
| Human | Domain knowledge | High (if expert) | Unavailable when unfamiliar |
| Agent self-assessment | Self-labeling uncertainty | **Low** | Agent doesn't know it's confabulating |
| **Reality** | **Run falsification experiment** | **High** | **Requires no domain knowledge from anyone** |

The key insight: **when neither human nor agent can judge, reality still can.** The human's
role is reduced to two zero-barrier tasks: (1) did the agent provide a falsification method?
(2) did it survive or fail?

## 5. Relationship

The three steps form a progressive filter, not alternatives:

| Step | Role | What it eliminates | Reliability |
|------|------|--------------------|----|
| Confirm | Self-consistency gate | Internally contradictory answers | Weak (can be gamed) |
| Corroborate | Independence check | Only-the-agent-says-so answers | Medium |
| **Falsify** | **Overturnability test** | **Answers that don't hold up to reality** | **Strong** |

- Confirm is the **gate** (filter out the absurd)
- Corroborate is a **bonus** (independent sources raise confidence)
- Falsify is the **core** (the only logically reliable filter)

All three are needed. Confirm without corroborate/falsify = self-serving. Falsify without
confirm = testing an incoherent hypothesis. Corroborate without falsify = groupthink.

## 6. Case Study

In a real investigation of GPU collective communication bandwidth (NCCL busbw exceeding
hardware spec), the agent produced 5+ wrong hypotheses. Each was eliminated by falsification:

| Agent's hypothesis | Falsification method | Result |
|--------------------|---------------------|--------|
| "busbw uses single-link spec" | Check 4-GPU intra-domain busbw | 135-182 vs spec 64 → **FALSIFIED** |
| "busbw = spec × algorithm factor" | Apply to AllGather (different factor) | Predicted 34, measured 67 → **FALSIFIED** |
| "Use P2P measured value as spec" | Check: is denominator theoretical or measured? | Measured ≠ theoretical → **FALSIFIED** |
| "Algorithm is Ring" | Run MCCL_DEBUG to inspect actual algorithm | Algorithm is Tree → **FALSIFIED** |
| "Dual-dimension evaluation method" | Search literature for this method | No literature basis → **FALSIFIED** (confabulated) |

**Every breakthrough came from falsification, never from confirmation.** The agent's self-
consistency checks (Step 2a) passed for every wrong hypothesis—it could always make the
numbers fit. Only independent corroboration (Step 2b) and falsification (Step 3) caught
the errors.

## 7. Failure Modes

Observed agent failure modes that this skill guards against:

1. **Numerical fitting ≠ physical understanding**: Seeing 67 ≈ 39×1.75 and constructing a
   "spec × factor" theory. Numbers coinciding is not causation.
2. **Framework lock-in**: Assuming "this is Ring algorithm" and modeling within that frame
   even after all models fail. Never questions the framework itself.
3. **Fabricating completeness**: When facing an open question, inventing a plausible-looking
   method with formulas and numbers rather than admitting "I don't know."
4. **Overconfident tone**: Using "fully consistent," "ironclad proof," "demonstrates" for
   unverified speculation. Tone is not evidence.
5. **Covering errors with new errors**: When one explanation is overturned, immediately
   producing another without acknowledging "I haven't figured this out yet."
