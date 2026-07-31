# AI Agent Skills

A public, vendor-neutral collection of reusable skills for AI coding agents, maintained by ZioNet and used in our own projects.

## Structure

Each directory under [`skills/`](skills/) is an independent Agent Skills-compatible package with a required `SKILL.md` file and optional references, scripts, examples, or assets.

## Install

GitHub CLI 2.90 or newer can preview and install a skill into the correct agent-specific location:

```text
gh skill preview Zio-Net/ai-coding-instructions SKILL_NAME
gh skill install Zio-Net/ai-coding-instructions SKILL_NAME --agent codex --scope project
gh skill install Zio-Net/ai-coding-instructions SKILL_NAME --agent claude-code --scope project
```

Skills can also be copied manually from `skills/`.

## License

MIT
