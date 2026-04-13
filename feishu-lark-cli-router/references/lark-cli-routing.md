# Lark CLI Routing Notes

Use these commands to orient quickly before doing anything custom.

## Install and setup detection

```bash
command -v lark-cli
command -v node
command -v npm
command -v npx
```

If `lark-cli` is missing, prefer:

```bash
npm install -g @larksuite/cli
npx skills add larksuite/cli -y -g
```

Then guide setup:

```bash
lark-cli config init --new
lark-cli auth login --recommend
lark-cli auth status
```

If a setup command returns a browser URL or device code, surface it to the user and continue after they complete the authorization step.

## Fast checks

```bash
lark-cli --help
lark-cli auth status
lark-cli doctor
```

## Domain discovery

```bash
lark-cli docs --help
lark-cli im --help
lark-cli calendar --help
lark-cli contact --help
lark-cli sheets --help
lark-cli base --help
```

## Method discovery

```bash
lark-cli schema docs.documents.create
lark-cli schema im.v1.message.create
```

Use `schema` when you need parameter names, required scopes, or a precise method path.

## Preferred escalation path

1. Shortcut command such as `lark-cli calendar +agenda`
2. Service API command such as `lark-cli calendar events instance_view --params '{...}'`
3. Raw API call such as `lark-cli api GET /open-apis/calendar/v4/calendars`

Before this escalation path, always ensure install and auth are healthy.

## Mapping from user intent

- "查飞书群消息": start with `lark-cli im --help`
- "查飞书文档": start with `lark-cli docs --help` or `lark-cli drive --help`
- "查飞书用户": start with `lark-cli contact --help`
- "查飞书日程": start with `lark-cli calendar --help`
- "查飞书表格或多维表格": start with `lark-cli sheets --help` or `lark-cli base --help`
- "查飞书会议纪要": start with `lark-cli vc --help` or `lark-cli minutes --help`
