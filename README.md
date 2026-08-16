# AI Agent Skills

A public, vendor-neutral collection of reusable skills for AI coding agents, maintained by ZioNet and used in our own projects.

## Skills

- [`docs-consistency-review`](skills/docs-consistency-review/) reviews documentation against code, configuration, tests, and related documents.
- [`design-lens-workshop`](skills/design-lens-workshop/) facilitates a focused, lens-driven technical design conversation before planning or implementation. It is a subtractive standalone adaptation of the [Specrew Design Workshop](https://github.com/alonf/specrew) method and nine-lens knowledge pack under the MIT License; it does not require Specrew and is not an official Specrew distribution.
- [`progressive-code-review`](skills/progressive-code-review/) runs an evidence-driven, staged branch review through risk-selected lenses, preserving decisions and surfacing design-shaping problems before dependent details.

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
