# pm-toolkit

A Claude Code plugin for product managers who build a prototype with Claude. It packages the workflow:

**connect tools → test the connections → create the PRD in Notion → build → test against the PRD → ship and mark "Built" in Notion**

It works for any project. Nothing in it is specific to one app.

## What's inside

| Part | What it does |
|---|---|
| `/pm-toolkit:connection-test` | Read-only check of which Notion, Figma and other tool accounts Claude is connected to. |
| `/pm-toolkit:new-prd <feature name>` | Adds the feature to your Notion "Features" database and puts `docs/PRD.md` on its page. |
| `/pm-toolkit:test-prd` | Tests every rule in the PRD in the running app and reports pass or fail. |
| Skill `ship-feature` | Builds, checks for secrets, commits, pushes, confirms the deployment and sets the feature to "Built" in Notion. |
| Notion and Figma connections | Added for you (`.mcp.json`). No keys are stored. |

Because the commands come from a plugin, their names start with `pm-toolkit:`.

## Install

You need Claude Code installed. This repository is both the plugin and a small marketplace that lists it.

**From GitHub** (anyone can do this), in a terminal:

```
claude plugin marketplace add rajanjinesh/pm-toolkit
claude plugin install pm-toolkit@pm-toolkit-local
```

Or, inside a Claude Code session:

```
/plugin marketplace add rajanjinesh/pm-toolkit
/plugin install pm-toolkit@pm-toolkit-local
```

**From a folder on your computer**: download or clone this repository, then use the folder's path instead, for example `claude plugin marketplace add D:\Projects\pm-toolkit`.

To try it for one session without installing, start Claude Code with the plugin folder:

```
claude --plugin-dir D:\Projects\pm-toolkit
```

After you change a file in the plugin, run `/reload-plugins` in the session.

The first time Notion or Figma is used, Claude Code asks you to sign in to it in your browser. Nothing is stored in the plugin.

## How to use it

Your project should have a `CLAUDE.md` that says where the PRD lives, which environment variable names and secret files it uses (names only), the Notion database name, how to build and run it, and where it's hosted. The `ship-feature` skill reads these from `CLAUDE.md` and asks you if something is missing.

1. **Check your tools.** Run `/pm-toolkit:connection-test`. It changes nothing. Fix anything that says "no" before you build.
2. **Put the PRD in Notion.** Write your PRD in `docs/PRD.md`, then run `/pm-toolkit:new-prd My feature name`. Claude finds or creates the "Features" database, adds a row with Status "Not started", and gives you the link. It doesn't write any app code.
3. **Build.** Build the feature with Claude as you normally would.
4. **Test it against the PRD.** Run `/pm-toolkit:test-prd`. Claude tests each rule (including any "Decisions" section), shows a pass/fail table, fixes failures, and tells you what to try yourself in the browser. It keeps calls to paid services low.
5. **Ship it.** Say "ship this feature", or run `/pm-toolkit:ship-feature My feature name`. Claude builds, checks no secrets are included, commits, pushes, asks you for the live URL if it can't see the host, then sets the feature to "Built" in Notion and adds a "Shipped" section with the URL and date.

## Good to know

- If Notion or Figma is already connected in your Claude Code settings, the plugin's copy of the connection may show up as a second entry. Both work; you can disable one.
- The plugin has no hooks and runs no code of its own. It only adds the commands, the skill and the two connections.

## Folder layout

```
pm-toolkit/
├── .claude-plugin/
│   ├── plugin.json         the plugin's name, version and author
│   └── marketplace.json    lets you install it from this folder
├── .mcp.json               Notion and Figma connections
├── commands/
│   ├── connection-test.md
│   ├── new-prd.md
│   └── test-prd.md
├── skills/
│   └── ship-feature/
│       └── SKILL.md
└── README.md
```
