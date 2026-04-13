---
name: feishu-lark-cli-router
description: Use this skill when the user asks to query Feishu/Lark data, users, docs, chats, calendars, sheets, base, approvals, mail, meetings, or other workspace content. Prefer the local lark-cli if present, and if it is missing, guide the user through local installation, app setup, authorization, verification, and then continue the Feishu task.
---

# Feishu Lark CLI Router

Use this skill whenever the user asks to "查询飞书", "查飞书", search Lark/Feishu, inspect workspace data, or operate on common Feishu domains such as docs, messages, calendar, contacts, sheets, base, wiki, tasks, mail, approvals, or meetings.

This skill is also responsible for first-run onboarding. If the recipient machine does not already have `lark-cli`, do not stop at "please install it". Act as a guided setup assistant: detect the missing dependency, help install it locally, walk through app configuration and login, verify the environment, and only then continue with the original Feishu request.

## Core Rule

Prefer the existing local CLI first:

```bash
~/.lark-cli/bin/lark-cli
```

Do not start by inventing a new integration flow or exploring the OpenAPI from scratch if the request can be served by `lark-cli`.

If `lark-cli` is missing, the first job is guided setup, not ad-hoc API exploration.

## Workflow

1. Detect whether `~/.lark-cli/bin/lark-cli` or `lark-cli` on `PATH` is available.
2. If the CLI is missing, guide setup locally:
   - Check whether `node`, `npm`, and `npx` exist.
   - Prefer `npm install -g @larksuite/cli`.
   - Prefer `npx skills add larksuite/cli -y -g` when skill installation is appropriate in that environment.
   - If installation from npm is not possible, explain the source-install fallback only then.
3. After install, help configure app credentials with `lark-cli config init` or `lark-cli config init --new`.
4. Help the user log in with `lark-cli auth login --recommend`.
5. If setup commands emit a browser verification URL or device code, surface it clearly to the user and wait for them to finish the browser step.
6. Verify readiness with `lark-cli auth status` or `lark-cli doctor`.
7. Once the environment is ready, inspect available commands with `lark-cli --help` and then the relevant domain help such as `lark-cli docs --help` or `lark-cli im --help`.
8. If the task needs a concrete API method, inspect it with `lark-cli schema <service.resource.method>`.
9. Use shortcut commands first when they fit because they are optimized for human and agent use.
10. If a shortcut is not enough, use the generated API command layer.
11. Only fall back to `lark-cli api <METHOD> <PATH>` when the higher-level command layers do not cover the request.

## Guided Setup Behavior

When `lark-cli` is absent or unauthenticated, stay in "onboarding mode" until the user is ready:

- Do the local checks yourself instead of asking the user to inspect their machine manually.
- Run the installation command for them when the environment permits.
- For interactive auth/config commands, explain that a browser action is required and relay any URL or code clearly.
- After each setup step, verify success before moving forward.
- Resume the original Feishu query automatically once setup is complete.

Use short, concrete language such as:

- "`lark-cli` is not installed locally yet, so I’m installing it first."
- "The CLI opened a browser authorization step; please finish that and I’ll continue."
- "Auth is ready now, so I’m switching back to your original Feishu query."

## Command Selection

Start from the domain that best matches the user's wording:

- Messages and chats: `lark-cli im ...`
- Docs: `lark-cli docs ...`
- Drive files: `lark-cli drive ...`
- Calendar: `lark-cli calendar ...`
- Contacts and users: `lark-cli contact ...`
- Sheets: `lark-cli sheets ...`
- Base: `lark-cli base ...`
- Wiki: `lark-cli wiki ...`
- Tasks: `lark-cli task ...`
- Mail: `lark-cli mail ...`
- Meetings or minutes: `lark-cli vc ...` or `lark-cli minutes ...`
- Approval flows: `lark-cli approval ...`

When unsure which command exists, use help instead of guessing:

```bash
lark-cli <service> --help
lark-cli schema <service.resource.method>
```

## Defaults

- Prefer `--format pretty` or `--format table` for user-facing inspection when supported by the command.
- Prefer `--format json` when the result needs filtering or follow-up processing.
- Use `--jq` for light extraction instead of writing extra scripts.
- Use `--page-all` only when the user actually wants exhaustive results.
- Prefer `--as user` or the default identity for "query my workspace" tasks unless the user explicitly needs bot behavior.
- During onboarding, prefer the least surprising happy path before presenting alternatives.

## Guardrails

- Do not expose secrets from `~/.lark-cli/config/config.json`.
- Do not overwrite an existing working auth or config unless the user asks you to fix setup.
- If a command fails, inspect help/schema/auth state before switching approaches.
- If the user only says "查询飞书的 XXX", interpret that as a request to use `lark-cli` first.
- If a shared skill is running on someone else's machine, do not assume any user-specific absolute path exists; detect local availability first and then use the actual installed path.

## Reference

For a compact routing cheat sheet, read [references/lark-cli-routing.md](references/lark-cli-routing.md) only when needed.
