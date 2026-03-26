# skill-creator-springett

A modified version of [Anthropic's skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator), rewritten through the lens of Jay Springett's ["Hard Worlds For Little Guys"](https://thejaymo.net/2026/03/19/hard-worlds-for-little-guys/).

The original skill-creator teaches you how to build skills. This version changes *how it thinks about what a skill is* — from a list of instructions addressed to a well-meaning actor to a world specification whose structure determines the quality of the actor's work.

## What changed

The eval/iterate loop, viewer infrastructure, packaging scripts, and description optimization are all preserved from the original. What changed is the skill writing guide and the improvement diagnostics.

### New concepts

**The world, not the wrapper.** A skill is a world the model wakes up inside. Tools become available actions, constraints become physics, context becomes the room. The central design move: promote constraints from advice into physics. A speed limit sign addresses the driver; a speed bump addresses the road. Build the bump.

**Three kinds of content.** Every line in a skill is one of three things:
- **Actor guidance** — judgment calls, style preferences, explanations of why something matters. Belongs in prose.
- **Room structure** — the local vocabulary of tools, files, and context currently in scope. The shape of the disclosed world.
- **World physics** — invariants that must hold regardless of the actor's memory. Completion conditions, forbidden actions, format requirements. Should be enforced by scripts and schemas, not sentences.

The most common authoring mistake is writing world physics as actor guidance. When you find yourself writing ALWAYS or NEVER in all caps, you're pleading with the actor about something the world should simply enforce.

**Dictionary discipline.** Tools, scripts, and references form the skill's dictionary. Composable primitives beat proliferating one-offs. Dictionary inflation — accumulating overlapping near-synonyms — degrades the actor's ability to choose correctly.

**Canonicalization at the boundary.** Accept flexible input, normalize early, work on the canonical form. Welcoming surfaces don't require soft internals.

**Staged commitment.** Propose → plan → execute. Don't collapse consequential action into one opaque step.

**Four-layer failure diagnostics.** When something goes wrong, diagnose which binding broke before rewriting prose:
- Lexical failure — couldn't find the right tool name
- Interface failure — right tool, wrong arguments
- World failure — action executed but produced wrong state
- Temporal failure — lost track of what already happened

### What's preserved

Everything mechanical: the eval runner, grading, benchmarking, the viewer, description optimization, packaging, the Claude.ai and Cowork adaptations. The improvement loop's numbered steps. The communication guidance about meeting users where they are.

## Installation

### Claude.ai
Upload `skill-creator/SKILL.md` as a custom skill, or upload the `.skill` package from [Releases](../../releases).

### Claude Code
```bash
# As a custom skill
cp -r skill-creator/ ~/.claude/skills/skill-creator-springett/

# Or install from the .skill package
claude skill install skill-creator-springett.skill
```

## Usage

Same as the original skill-creator. Ask Claude to help you build, improve, or evaluate a skill. The differences are in how it thinks about skill design, not in what commands you run.

## Lineage

**Base skill:** [anthropics/skills/skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator) — Apache 2.0

**Theoretical framework:** Jay Springett, ["Hard Worlds For Little Guys"](https://thejaymo.net/2026/03/19/hard-worlds-for-little-guys/) — CC BY-SA 4.0. The essay argues that LLM agents become reliable not through better instructions but through hard worlds where tools, constraints, state, and consequences are built into the environment as world physics. It draws on fifty years of interactive fiction and MUD design to supply vocabulary (rooms, parsers, dictionaries, exits, gates, flight envelopes) for analyzing agent harnesses as ontological structures. The essay's direct critique of SKILL.md as a format — that it flattens identity, procedure, stop conditions, and completion criteria onto one undifferentiated prose surface — motivated this rewrite.

## A/B testing

If you want to compare this against the original, install both:

```
skills/
├── skill-creator/           # Anthropic's original
└── skill-creator-springett/ # This version
```

Then try building the same skill with each and compare the outputs. The interesting differences should show up in how the skill *structures* constraints (prose wishes vs. enforced scripts), how it diagnoses failures during iteration, and whether the resulting skills are leaner.

## License

Apache 2.0 (following the original skill-creator).
