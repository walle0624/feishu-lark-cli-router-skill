# feishu-lark-cli-router-skill

Shareable Codex skill for Feishu/Lark queries.

This repository contains a reusable skill that teaches Codex to:

- route Feishu/Lark requests to `lark-cli` first
- detect when `lark-cli` is missing
- guide local installation and authorization
- continue the original Feishu task after setup succeeds

## Repository Contents

```text
feishu-lark-cli-router/
  SKILL.md
  agents/openai.yaml
  references/lark-cli-routing.md
```

## Install

Copy the `feishu-lark-cli-router` directory into:

```bash
~/.codex/skills/
```

Result:

```bash
~/.codex/skills/feishu-lark-cli-router/
```

Then start a new Codex conversation.

## Example Prompts

```text
查询飞书的文档
查一下飞书群消息
帮我查询 Feishu 用户
看下飞书日程
```

## Privacy

This repository is intentionally sanitized for sharing:

- no local auth files
- no tokens or secrets
- no personal account metadata
- no machine-specific absolute home directory paths

The skill only references standard local locations such as `~/.lark-cli/...` when describing expected CLI behavior.
