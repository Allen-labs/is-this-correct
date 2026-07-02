# is-this-correct

> **Keep doubting: falsify before you trust.**

A cross-platform skill for AI coding agents (Codex, Claude Code, OpenCode, ZCode) that
enforces **falsifiability-driven verification** for non-trivial conclusions—preventing
the agent from presenting plausible-but-wrong answers as settled truth.

## The problem it solves

AI agents are induction machines: they generate plausible hypotheses but cannot reliably
tell correct from plausible-but-wrong. When they confabulate, they genuinely believe they
are right—and state unverified speculation with the same confidence as established fact.

When a human works with an agent in a domain they don't master, they cannot act as judge.
The result: plausible-but-wrong answers get accepted as truth.

## How it works

Based on Karl Popper's philosophy of science, the skill forces a four-step loop for every
non-trivial conclusion:

```
Step 1  Conjecture           Formulate hypothesis + theoretical basis
Step 2  Confirm + Corroborate Self-consistency gate + independent sources
Step 3  Falsify              Try to prove yourself wrong — let reality judge
Step 4  Adjudicate           Assign confidence: High / Medium / Low / Open
```

**Core principle**: A conclusion is trustworthy because it *survived attempts to overthrow
it*, not because it sounds right. The judge is reality (data, code, docs, tools)—not the
human (who may not know the domain) and not the agent (who can't self-assess).

## Installation

### Codex / ZCode

```bash
cp -r is-this-correct/ ~/.codex/skills/
```

### Claude Code (as plugin)

```bash
# Clone and add to Claude Code plugins
gh repo clone <your-account>/is-this-correct
# Or copy the plugin manifest + skills directory
cp -r .claude-plugin/ is-this-correct/ ~/.claude/plugins/local/is-this-correct/
```

### OpenCode

```bash
cp -r is-this-correct/ ~/.config/opencode/skills/
```

## Project structure

```
is-this-correct/
├── README.md                              This file (repo-level)
├── LICENSE                                MIT
├── .claude-plugin/
│   └── plugin.json                        Claude Code plugin manifest
└── is-this-correct/                       Skill directory (install this)
    ├── SKILL.md                           Core skill definition
    ├── agents/
    │   └── openai.yaml                    Codex/ZCode UI metadata
    └── references/
        └── methodology.md                 Theory + case study (loaded on demand)
```

## Key rules enforced

1. Never present an unfalsified hypothesis as a settled conclusion
2. "Survived" ≠ "proven correct" — phrase as "not yet overturned"
3. Confabulation alert: flag self-constructed metrics/formulas with no external basis
4. 3-strike rule: after 3 consecutive falsifications, stop and question the framework
5. Use tools for ground truth when reasoning stalls

## Theoretical basis

- **Karl Popper** — *The Logic of Scientific Discovery*: science doesn't confirm, it
  falsifies; surviving theories are "not yet overturned," not "proven."
- **David Hume** — Problem of induction: finite observations cannot establish universal
  claims.

See [`is-this-correct/references/methodology.md`](is-this-correct/references/methodology.md)
for the full background and a real case study.

## License

MIT
