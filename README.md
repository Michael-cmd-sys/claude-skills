# claude-skills

A collection of globally shareable skills for Claude Code agents.
Built on the belief that knowledge compounds when it's shared — society made it this far because people shared what they learned.

## Skills

### `project-setup`
Opinionated project scaffolding for any stack. Creates production-ready layouts with proper separation of concerns, config files, dependency management, and developer tooling. Covers web apps, REST APIs, CLIs, libraries, monorepos, and full-stack projects.

### `adaptive-learning`
A self-improvement framework for AI models. Guides models to detect signals worth keeping, learn from user preferences and corrections, build a core philosophy, and apply accumulated lessons across future sessions. The goal: not more rules, but better judgment.

---

## Install All Skills

```bash
npx skills add michaelawunikofi/claude-skills
```

## Install Individual Skills

```bash
npx skills add michaelawunikofi/claude-skills@project-setup
npx skills add michaelawunikofi/claude-skills@adaptive-learning
```

## What Are Claude Code Skills?

Skills are modular knowledge packages that extend Claude Code's capabilities.
They live in `~/.claude/skills/` and are automatically loaded into every session.

Browse the ecosystem: https://skills.sh

---

## Contributing

If a skill helped you, or you've learned something worth sharing — open a PR.
The format is simple: a directory with a single `SKILL.md` file.

```
your-skill-name/
└── SKILL.md   ← frontmatter (name, description) + skill instructions
```
