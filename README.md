# mini-Brain Skills

A set of [Claude Code Skills](https://docs.claude.com/en/docs/claude-code/skills) modeled after distinct human thinking patterns. Each skill captures one specific cognitive mode — divergent ideation, convergent reasoning, elaboration, curious exploration, production, teaching, learning, and quick experimentation — as its own standalone, invocable skill.

Built with and for [Claude Code](https://claude.com/claude-code) by Anthropic.

## Skills

| Skill | Thinking mode | What it does |
|---|---|---|
| [`/brainstorm`](brainstorm/SKILL.md) | Divergent ideation | Generates a spray of ideas across three energy tiers (🔥 Wild / 💡 Core / 🌱 Seed), judgment suspended |
| [`/thinking`](thinking/SKILL.md) | Convergent reasoning | Works a decision or question through a visible step-by-step chain to a defensible conclusion |
| [`/idea`](idea/SKILL.md) | Elaboration | Takes ONE fixed idea and fleshes it out into something concrete — shape, dependencies, sharpest edge |
| [`/explore`](explore/SKILL.md) | Curious mapping | Wanders an unfamiliar topic laterally, building a mental map without forcing a conclusion |
| [`/create`](create/SKILL.md) | Production | Commits to one direction and produces the actual finished artifact — no alternatives, no hedging |
| [`/guide`](guide/SKILL.md) | Mentorship | Breaks a process into a walkable, checkpointed sequence of actions the user performs themselves |
| [`/study`](study/SKILL.md) | Assimilation | Builds a durable mental model of a concept, anchored to what you already know, verified with a self-check |
| [`/try`](try/SKILL.md) | Experimentation | Produces one rough, disposable attempt fast, to generate a signal rather than a finished answer |

## Why separate skills, not one combined skill

Each thinking mode has a genuinely different shape, pace, and output format — a divergent idea spray and a convergent reasoning chain don't compose into one instruction set without diluting both. Keeping them as standalone skills lets each one trigger precisely on its own intent and stay sharp.

## Repo structure

```
<skill-name>/
├── SKILL.md            # required: YAML frontmatter (name, description) + instructions
└── evals/
    └── evals.json       # optional test cases for that skill
```

See [CLAUDE.md](CLAUDE.md) for authoring conventions and the eval workflow used while developing these skills.

## Using a skill

Drop any `<skill-name>/` directory into your Claude Code skills path, or reference `SKILL.md` directly. Each skill's frontmatter `description` is its trigger — Claude Code decides when to consult it based on that description matching the task at hand.
