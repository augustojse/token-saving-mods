# Token-saving mods

Nine Claude Code mods that cut token cost, plus a `/mods` switchboard and a `/savings` pane. This repo is a Claude Code plugin marketplace named `token-saving-mods`.

Most mods use Haiku (the cheaper model) to read or judge things first, so the main model sees less. They run through hooks, so they work on their own at set moments: a prompt, a big tool result, a subagent spawn.

## The mods

| Mod | What it does | Commands |
|---|---|---|
| `input-router` | Big file reads and connector results are read by Haiku first. Claude gets short notes and a path to the full thing. A side pane shows each hand-off. | `/input-router full` lets the next one through whole |
| `effort-optimizer` | Reads each prompt, picks the effort level it needs, and offers to switch (or switches by itself in auto mode). | |
| `model-router` | At subagent spawn, Haiku judges the task and may move it to a cheaper model when confident enough. Never changes the main chat's model. Logs picks to `~/.claude/mods-logs/savings.jsonl`. | |
| `cache-clock` | Counts down until the prompt cache goes cold, above the prompt. Offers a handoff when a big chat goes cold. "Keep warm" pings the cache. | |
| `handoff` | At about 250k tokens, offers Refresh: writes a handoff of the chat, then copies it or starts a new chat with it. Logs to `~/.claude/handoffs/handoff.log`. | |
| `research-router` | Haiku shortens Claude's own big search results. A cheap worker or subagents read real pages and return a short brief with sources. | `/research-router` |
| `efficient-browser-use` | Shrinks screenshots Claude takes while driving a browser, or has Haiku describe them in words. | `/efficient-browser full` lets one full-size screenshot through |
| `mods` | A Mods pane that finds every mod on the computer and turns each on or off in every chat. | `/mods` |
| `savings-proof` | Shows the tokens the other mods saved, with today's and this chat's totals. | `/savings` |

## Requirements

- Claude Code 2.1.287 or newer in the terminal, or 2.1.286 or newer in the desktop app.
- Windows 10 or 11 with Windows PowerShell 5.1 (built in). The pack was built for Windows and has not been fully tested there. It may not work on Linux or macOS.

Optional extras (the mods work without them):
- `input-router` reads PDFs only if [Poppler for Windows](https://github.com/oschwartz10612/poppler-windows) is installed and its `bin` folder is on your PATH.
- `research-router` uses Firecrawl only if the Firecrawl CLI is installed with npm.

On Windows, WebP pictures are not made smaller. PNG and JPEG pictures are.

## Install

Run these in Claude Code, or use `claude plugin ...` in a terminal:

```
/plugin marketplace add augustojse/token-saving-mods
/plugin install input-router@token-saving-mods
/plugin install effort-optimizer@token-saving-mods
/plugin install model-router@token-saving-mods
/plugin install cache-clock@token-saving-mods
/plugin install handoff@token-saving-mods
/plugin install research-router@token-saving-mods
/plugin install efficient-browser-use@token-saving-mods
/plugin install mods@token-saving-mods
/plugin install savings-proof@token-saving-mods
/reload-plugins
```

Installed at user scope, the mods load in every Claude Code session on that computer. Type `/mods` to see them and `/savings` to see the totals.

`model-router` has optional settings. Run `/plugin configure model-router@token-saving-mods` to set them, or keep the defaults.

To install from a local copy of this repo instead, replace the first line with `/plugin marketplace add <path to this folder>`. The folder must contain `.claude-plugin`.

## Cloud and project use

`.claude/settings.json` in this repo registers the marketplace and enables all nine mods. A Claude Code session started on this repo should offer to install them. To use the mods in another project, copy those two blocks (`extraKnownMarketplaces` and `enabledPlugins`) into that project's `.claude/settings.json`.

Cloud sessions run on Linux and are separate from your computer, so some mods may not work there and `/savings` shows only that session's activity.

## Turn mods on or off

- Use the `/mods` pane, which applies in every chat.
- Or run `claude plugin disable <name>@token-saving-mods` / `claude plugin enable <name>@token-saving-mods`, then `/reload-plugins`.

## Update

```
/plugin marketplace update token-saving-mods
/reload-plugins
```

## Troubleshooting

- `marketplace add` cannot find the folder: the path you give must contain the `.claude-plugin` folder.
- A mod misbehaves: turn it off in `/mods`. The others keep working.
- The Mods pane's zip-upload button won't take this pack, because it holds nine plugins and the button takes one. Use the marketplace steps above.

## Credits and licenses

Pack from Ben AI (https://www.benai.co). `model-router` is a changed copy of the Jev Model Router from [claude-code-templates](https://github.com/davila7/claude-code-templates) (MIT; see `plugins/model-router/LICENSE`). Check each plugin's own folder for its license and README before sharing the pack.
