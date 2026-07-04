# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A collection of Claude Code Skills modeled after human thinking patterns (e.g. `/brainstorm`, with `/explore`, `/thinking`, `/idea`, `/thoughts`, `/try`, `/create`, `/guide`, `/study`, `/reframe` planned). Each skill is a standalone directory, built and tested independently — not one combined skill.

## Repo structure

```
<skill-name>/
├── SKILL.md            # required: YAML frontmatter (name, description) + markdown instructions
└── evals/
    └── evals.json       # test cases for that skill
```

There is no build system, package manager, or source code to compile — skills are markdown instruction files consumed directly by Claude Code's skill-loading mechanism.

## Skill authoring conventions (established in `brainstorm/SKILL.md`)

- **Frontmatter description is the trigger mechanism.** It must state both what the skill does and when to use it, phrased "pushy" enough that Claude doesn't undertrigger it (list explicit trigger phrases, e.g. "/brainstorm", "let's brainstorm", "give me ideas for").
- **Body structure**: what the skill is doing conceptually → how to read/interpret user input → exact output format (use literal template blocks) → tone/energy guidance → explicit anti-patterns to avoid → optional user-directed "modes" → worked examples.
- Keep SKILL.md under ~500 lines.

## Testing skills (eval workflow)

Test cases live at `<skill-name>/evals/evals.json`:

```json
{
  "skill_name": "example-skill",
  "evals": [
    {
      "id": 1,
      "prompt": "User's example prompt",
      "expected_output": "Description of expected result",
      "files": [],
      "expectations": ["Verifiable statement 1", "Verifiable statement 2"]
    }
  ]
}
```

The full eval workflow (spawn with-skill + baseline subagent pairs, grade against `expectations`, aggregate into `benchmark.json`, launch `eval-viewer/generate_review.py` for human review, iterate) is driven by the `skill-creator` skill/plugin, not by any script in this repo. Invoke that skill (or read its `SKILL.md` / `references/schemas.md`) rather than reinventing the eval harness here.

Workflow per skill: draft SKILL.md → write 2-3 realistic test prompts → run with-skill vs baseline in parallel → grade against `expectations` → review via eval viewer → iterate on SKILL.md → repeat → package as `.skill` when satisfied.
