---
name: ship-feature
description: Wraps up a finished feature. Use when asked to ship, publish or wrap up a feature, or after a feature works locally.
argument-hint: [feature name]
---

# Ship Feature

## First, read the project's CLAUDE.md

Don't hard-code anything. Find these in the project's `CLAUDE.md` (and `package.json` for commands):

- **Feature name:** the name I typed after the command: $ARGUMENTS. If I typed nothing, use the feature that CLAUDE.md or the Notion Features database shows as "In progress". If it's unclear, ask me.
- **Secrets:** the names of the environment variables and local secret files (such as `.env.local`) the project uses. You only ever need their names, never their values.
- **Notion database:** the name of the Notion database that holds the features, and its Status values.
- **Build and run commands, hosting platform and main branch:** as written in CLAUDE.md or `package.json`.

If something you need isn't there, ask me. Don't guess.

## Steps

Run these steps in order. Stop and tell me if any step fails.

1. **Check it works.** Run the project's build command and fix any errors. Confirm the feature works with the project's local run command.
2. **Check nothing secret is included.** Make sure the secret files and the environment variables named in CLAUDE.md are not staged or printed anywhere.
3. **Commit.** Use a clear message that says what the feature does, e.g. "feat: show top 3 cheapest results".
4. **Push to GitHub** (the main branch named in CLAUDE.md). The hosting platform deploys from it automatically.
5. **Confirm the deployment.** Check that the deployment for this commit is live on the hosting platform. If it failed, report why.
   - If you can't check the hosting platform directly, ask me for the live URL. Then open it and confirm the app loads and its main screen shows.
   - If the project isn't deployed yet, tell me to import the repo in the hosting platform's dashboard and add the environment variables named in CLAUDE.md. Then wait for me to give you the URL.
6. **Update Notion.** Set the feature's Status to "Built" in the Notion database named in CLAUDE.md. Also add the live URL and today's date to the feature's Notion page under a "Shipped" heading.

Finish with a summary report in plain language, in this format:

| Step | Status | Detail |
|---|---|---|
| Build and local check | ✅ PASS / ❌ FAIL | e.g. "built with no errors" |
| Secrets check | ✅ PASS / ❌ FAIL | e.g. "secret files not committed" |
| Commit | ✅ PASS / ❌ FAIL | commit message and short hash |
| Push to GitHub | ✅ PASS / ❌ FAIL | branch name |
| Deployment | ✅ PASS / ❌ FAIL | Ready / Failed, and why if it failed |
| Notion Status | ✅ PASS / ❌ FAIL / ⏭️ SKIPPED | "Built", or why it was skipped |

- **Live URL:** the deployed link, or "not deployed" if it failed.
- **Open items:** anything unfinished or worth checking, or "none".

If a step failed, mark it ❌, mark later steps ⏭️ SKIPPED, and say what to fix.

Rules:
- Never commit secret files or print any key or token.
- Don't set Status to "Built" until the deployment is live.
- Don't add new features while shipping; note new ideas under "Future scope" in the PRD.
