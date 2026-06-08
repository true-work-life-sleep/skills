---
name: dev-fast
description: Orchestrates a full feature implementation cycle from Jira/Confluence card to committed code. Reads ticket, grills requirements, sets up git branch, implements code, writes unit tests, verifies with npm run test and npm run dev, then commits. Use when user says "dev-fast", wants to implement a Jira card end-to-end, or wants to start development from a ticket.
---

# dev-fast

Implement a Jira card end-to-end: read ticket → understand requirements → branch → code → test → commit.

## Step 0 — Check and install prerequisites

Check whether the following skills are installed:
- `~/.agents/skills/jira-integration/SKILL.md`
- `~/.agents/skills/grill-me/SKILL.md`

If missing, install them automatically:
```bash
npx skills@latest add mattpocock/skills
npx skills add affaan-m/ECC --skill jira-integration
```
Once installed, proceed to Step 1.

## Step 1 — Read Jira card

Activate the **jira-integration** skill and read the content from the Jira card or Confluence page specified by the user.
Collect: Jira card ID, card name, description, acceptance criteria, and work type (feature/enhance/hotfix/bug/other).

## Step 2 — Understand requirements

Activate the **grill-me** skill to ask clarifying questions and build a solid understanding before planning.
Produce a clear implementation plan once grilling is complete.

## Step 3 — Set up git branch

1. Verify the working directory is the correct project — if not, **stop and inform the user**.
2. `git checkout develop && git pull origin develop`
3. Create a new branch using the format:
   - `feature/{jira-card}-{jira-name-card}`
   - `enhance/{jira-card}-{jira-name-card}`
   - `hotfix/{jira-card}-{jira-name-card}`
   - `bug/{jira-card}-{jira-name-card}`
   - `other/{jira-card}-{jira-name-card}`
4. `git push -u origin {branch-name}`

## Step 4 — Implement

Compare the plan against the actual codebase in the working directory, then begin implementation.

**Env rule**: If `conf/application.json` requires changes, only modify values inside the `test`, `sit`, and `uat` keys. Skip any key that does not exist.

**Legacy error handling rule**: When adding or modifying calls to a legacy node, apply this rule:

- **Break-flow legacy** — the API requires data from the legacy node to continue (e.g. must process its response before proceeding):
  - Any error or unexpected response → throw error, stop the API flow.
- **Non-break-flow legacy** — the API can continue without the legacy node's data (e.g. result is passed to another node as optional info):
  - **Do not** rely on HTTP status alone. Each legacy node has its own contract — a "not found" may return 404, or 200 with an empty/null data field. Read the Confluence doc for that legacy call to understand what request it expects and what response shapes it returns (found / not found / error).
  - "Not found" response (as defined by the contract) → handle gracefully; do not throw.
  - Unexpected error (server fault, timeout, contract-violating response) → still throw.

**Legacy layer ownership rule**: All error/contract checks for a legacy node belong inside `apps/legacy/*.js`, not in the service layer. The legacy file is the single source of truth for that external service's contract — it must throw on any error condition and return clean data on success. The service layer only maps and orchestrates.

## Step 5 — Unit tests

- New API endpoint → write unit tests with 100% coverage.
- Modified existing API endpoint → update unit tests to maintain 100% coverage.

## Step 6 — Run tests

Run `npm run test` and verify that all implemented APIs have 100% coverage.
If not, fix and return to Step 5.

## Step 7 — Runtime check

Run `npm run dev` and wait 10 seconds:
- **No crash** → Ctrl+C and proceed to Step 8.
- **Crash from code** → analyze the error, plan a fix, return to Step 4.
- **Crash from external factors** (DB, Redis, service connection, etc.) → report the error details to the user and stop.

## Step 8 — Commit & push

```bash
git add .
git commit -m "{jira-card}: {AI-written message summarizing what was implemented}"
git push origin {branch-name}
```

Write a commit message that reflects the actual changes made, e.g. `PROJ-123: add user login API with JWT authentication`.

Notify the user: **"implement {jira-card}-{jira-name-card} done"**
