---
description: Verify a claim using the falsifiability loop. Pass a claim as argument (e.g. /itc 0.1+0.2==0.3 is True).
---

# /itc

Verify the user's claim or question using the four-step loop defined in `SKILL.md`:
**Conjecture → Confirm+Corroborate → Falsify → Adjudicate**.

Run the full loop from `SKILL.md` (read it if not already loaded), following all rules
and the output template specified there.

## Slash-command specifics

- Treat the argument as the claim to verify.
- If the argument is a question ("how does X work?"), first extract a falsifiable claim
  from it, then verify that claim.
- Always show the structured output from `SKILL.md`'s template, even if the result is Open.
