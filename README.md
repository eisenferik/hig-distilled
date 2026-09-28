# HIG Distilled

An explicit-invocation Agent Skill for applying Apple's Human Interface Guidelines to web
and cross-platform interface work.

HIG Distilled teaches an agent to reason with HIG-derived design principles: purpose, agency,
familiarity, flexibility, simplicity, craft, and delight. It covers visual systems,
navigation, controls, interaction states, writing, accessibility, dark mode, and
Apple-style web implementation guidance.

It does not clone Apple interfaces pixel-for-pixel or bundle Apple fonts or icons.

## Install

Using the Agent Skills CLI:

```sh
npx skills add eisenferik/hig-distilled --skill hig-distilled
```

For manual installation, copy the complete `skills/hig-distilled` directory into your agent's
skills directory. Keep its `references/` and `assets/` folders with `SKILL.md`.

## Use

Invoke the skill explicitly:

```text
Use hig-distilled to design this settings screen.
```

```text
Review this interface using Apple's Human Interface Guidelines.
```

The skill intentionally does not activate for generic UI, UX, polish, or design requests
that do not ask for HIG or an Apple, iOS, or macOS direction.

## Compatibility

The skill follows the Agent Skills `SKILL.md` format and is intended for Codex, Claude Code,
and other compatible coding agents. `agents/openai.yaml` provides optional Codex-facing
display metadata; other agents can ignore it.

## Scope

HIG Distilled is an independent, paraphrased distillation of design guidance. It is not affiliated
with or endorsed by Apple and does not replace the official Human Interface Guidelines.

## License

MIT. See [LICENSE](LICENSE).
