# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A collection of Claude Code Skills modeled after distinct human thinking patterns. Each skill is a standalone directory, built and tested independently — not one combined skill. See [README.md](README.md) for the full skill table and rationale.

Built: `/brainstorm` (divergent ideation), `/thinking` (convergent reasoning), `/idea` (elaboration), `/explore` (curious mapping), `/create` (production), `/guide` (mentorship/process teaching), `/study` (assimilation/learning), `/try` (quick experimentation).

## Repo structure

```
<skill-name>/
├── SKILL.md            # required: YAML frontmatter (name, description) + markdown instructions
└── evals/
    └── evals.json       # optional: test cases for that skill
```

There is no build system, package manager, or source code to compile — skills are markdown instruction files consumed directly by Claude Code's skill-loading mechanism.

`agent-skills-docs.md` at repo root is a local reference copy of Anthropic's official Agent Skills docs — consult it for the underlying skill-loading mechanism (progressive disclosure, frontmatter requirements) rather than relying on memory.

## Keep README.md in sync

**Whenever a skill is added, removed, or its description/trigger scope changes materially, update the skill table in [README.md](README.md) in the same change.** README is the discovery surface (skill list, thinking-mode mapping, rationale) — letting it drift from the actual skill set defeats its purpose. This applies even for changes that don't touch CLAUDE.md itself.

## Skill authoring conventions (established in `brainstorm/SKILL.md`, followed by all skills since)

- **Frontmatter description is the trigger mechanism.** It must state both what the skill does and when to use it, phrased "pushy" enough that Claude doesn't undertrigger it — list explicit trigger phrases (e.g. "/brainstorm", "let's brainstorm", "give me ideas for").
- **Cross-skill disambiguation lives in the frontmatter too, not just the body.** Skill matching keys off the description text. Each skill's description names its nearest siblings and states the boundary (e.g. `/study`'s frontmatter explicitly distinguishes itself from `/guide` and `/explore`). Body prose alone won't stop a collision at match time.
- **Before adding a new skill, grep existing frontmatters for shared trigger phrases**: `grep -rn "<candidate phrase>" */SKILL.md`. A literal duplicate phrase across two skills' trigger lists undermines both, even when the surrounding prose disambiguates — this has happened once already (`/explore` and `/study` both listed "help me understand" verbatim; fixed by rewording).
- **Body structure**: what the skill is doing conceptually → how to read/interpret user input → exact output format (use literal template blocks) → tone/energy guidance → explicit anti-patterns to avoid → optional user-directed "modes" → worked examples (2, in the same literal-template style) → closing section on what makes this different from just doing the task without the skill.
- Keep SKILL.md under 500 lines; most sit in the 90-200 line range.

## Testing skills (eval workflow — optional, not required per skill)

Test cases, when written, live at `<skill-name>/evals/evals.json`:

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

Only `brainstorm` and `thinking` currently have evals — the rest were built and reviewed conversationally (draft → read → iterate) without the eval harness. Default to the lightweight conversational loop unless evals are explicitly requested.

When evals are wanted, the full workflow (spawn with-skill + baseline subagent pairs, grade against `expectations`, aggregate into `benchmark.json`, launch `eval-viewer/generate_review.py` for human review, iterate) is driven by the `skill-creator` skill/plugin, not by any script in this repo — invoke that skill rather than reinventing the harness. Note: `eval-viewer/generate_review.py` requires Python 3.10+ (uses `X | None` union syntax); if only 3.9 is available, skip the browser viewer and present eval outputs directly in conversation instead.
