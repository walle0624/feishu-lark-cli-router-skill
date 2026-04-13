# feishu-lark-cli-router-skill

A shareable Codex skill that routes Feishu/Lark requests to `lark-cli` first, and guides local setup when the CLI is missing.

## Why This Exists

When people ask Codex to "query Feishu" or "check Lark", the agent can waste time rediscovering tooling or inventing an integration flow from scratch.

This skill gives Codex a stable default:

- prefer `lark-cli` for Feishu/Lark work
- detect whether the CLI is installed locally
- guide installation and authorization when it is not
- continue the original task after setup is complete

## What The Skill Covers

- Feishu docs and drive files
- chats and messages
- calendars and schedules
- contacts and users
- sheets and base
- wiki, tasks, mail, approval, meetings, and minutes
- first-run local setup for `lark-cli`

## Repository Layout

```text
feishu-lark-cli-router/
  SKILL.md
  agents/openai.yaml
  references/lark-cli-routing.md
```

## Install

Copy the `feishu-lark-cli-router` directory into your local Codex skills directory:

```bash
mkdir -p ~/.codex/skills
cp -R feishu-lark-cli-router ~/.codex/skills/
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
查询飞书多维表格里的记录
```

## Behavior

The skill is designed to be operational instead of advisory.

If `lark-cli` is already available, Codex should use it first.

If `lark-cli` is missing, Codex should:

1. detect the missing dependency
2. guide local installation
3. walk through Feishu app setup and login
4. verify the environment
5. resume the original Feishu task

## Privacy And Safety

This repository is intentionally sanitized for sharing:

- no local auth files
- no tokens or secrets
- no personal account metadata
- no machine-specific absolute home directory paths

The skill references standard local locations such as `~/.lark-cli/...` only as expected CLI conventions.

## Compatibility

This project assumes:

- Codex supports local skills from `~/.codex/skills/`
- the target user can run local shell commands
- `lark-cli` is either already available or can be installed locally

## Contributing

Issues and pull requests are welcome.

Please keep changes aligned with the core goal of this repository:

- help Codex choose `lark-cli` early
- improve first-run onboarding
- keep the skill safe to share publicly
- avoid embedding machine-specific or user-specific secrets

See [CONTRIBUTING.md](CONTRIBUTING.md) for a lightweight contribution guide.

## License

MIT. See [LICENSE](LICENSE).
