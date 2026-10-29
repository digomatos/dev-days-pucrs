---
title: "Lesson 9 - Explore and create canvases"
description: "Use the existing Database Explorer canvas, then create and review a repository-backed triage canvas."
authors:
  - geektrainer
lastUpdated: 2026-09-17
---

So far you've directed agents through chat. But a lot of work doesn't live in a conversation — it lives on a board, in a document, or on a checklist. **Canvases** give you and the agent a shared surface for exactly that kind of work, right inside the app. In this lesson you'll first use a canvas included with Tailspin Toys, then create one for the backlog you've been working through.

In this lesson, you will:

- understand what a canvas is and when to use one.
- use the existing Database Explorer canvas to inspect project data.
- create a shared Kanban board canvas to triage your backlog.
- inspect and exercise the new canvas without implementing another feature.

## Scenario

Tailspin Toys already includes a canvas for exploring its database. After using it to understand how a canvas turns project data into an interactive surface, you'll create a reusable board for choosing what to work on next without starting another feature.

## What is a canvas?

A [canvas][canvas-docs] is a shared, interactive surface for a work artifact — a plan, a triage board, a release checklist, a dashboard, or a document. While chat is useful for describing intent and reasoning through ambiguity, most work happens on a *surface*. Canvases let you collaborate with the agent directly on that surface.

Canvases are **bidirectional**: the agent can update the canvas while it works, and you can edit the same surface yourself. When you create a canvas, the agent builds it based on your prompt and workflow, and you can ask it to add, remove, or revise capabilities as you go. Once created, a canvas opens in the app's right side panel.

Some common examples include:

- **Markdown canvases** for planning your day and prioritizing issues and pull requests.
- **Agentic Kanban boards** where people and agents add cards and move work across columns.
- **Issue triage boards** that summarize top issues and recurring themes for a repository.

## Why use a canvas?

Reach for a canvas when a task needs structure, iteration, and verification, and a chat alone isn't enough. A canvas lets you:

- ground the agent's work in an actual artifact that fits your workflow.
- steer or correct work directly on the shared surface, then let the agent continue from your changes.
- inspect progress as visible changes to an artifact, not just chat responses.

## Use the Database Explorer canvas

Start with the project's existing Database Explorer canvas. Using a working example lets you see how a repository-scoped canvas behaves before you create one yourself.

### Prepare and open the canvas

1. Confirm the filtering pull request (PR) is merged and update your local `main`.
2. Return to the GitHub Copilot app and select **+** next to **Sessions** in the left sidebar to create a new session.
3. Select the **tailspin-toys** repository.
4. Create the session in **New worktree**, using the updated `main` as the source branch.
5. Select **Interactive** mode, then ask Copilot to prepare the dependencies and local database using the project's scripts:

    ```plaintext
  Inspect the project's package scripts, install its dependencies, and use its existing database setup and seed scripts if the local database has not already been initialized. Preserve any existing database and its contents. Check that the scripts and the existing Database Explorer extension use the same database. You may install dependencies and configure necessary local files, but do not modify application or extension code, overwrite the database, commit, push, or open a pull request. Report what you ran, the database paths, and any blockers before opening the canvas.
    ```

6. Review the script output and database paths. The lab used the project's dependency installation and `npm run db:setup` scripts. Inspect the scripts before running setup or seed commands against an existing database; do not reset or overwrite it. If loading fails or the database paths differ, follow [Troubleshoot the Database Explorer](#troubleshoot-the-database-explorer) before continuing.
7. After installing dependencies and configuring the local database path, run `/restart-session` to reload the extension in the same session, preserving its history and the same worktree. Do not create another session or worktree.
8. Ask Copilot to open the existing canvas and check its tables and data:

  ```plaintext
  Open the repository's existing Database Explorer canvas. Use its read-only actions to list the tables and verify the seeded data. Do not modify application or extension code, overwrite the database, commit, push, or open a pull request.
  ```

9. Visually confirm that the Database Explorer panel opens and displays its tables and data. For the seed used in the lab, the expected result was **21 games, 5 categories, and 4 publishers**. The `__drizzle_migrations` table also appeared. Select `games` and inspect its rows; use the canvas's read-only actions to check the counts. Investigate differences rather than overwriting an existing database to force a match.
10. Run a read-only query that shows five highly rated games:

    ```sql
    SELECT title, star_rating
    FROM games
    ORDER BY star_rating DESC
    LIMIT 5;
    ```

  11. Confirm the results contain no more than five games in descending rating order.
  12. Open **Files** and inspect `.github/extensions/database-explorer/extension.mjs`. Note how the canvas is stored with the project and restricts queries to read-only `SELECT` and `WITH` statements.
  13. Review the local changes and confirm that application and extension code remain unchanged. Dependency installation and local database configuration may create local files; do not commit, push, or open a PR for this setup.

  > [!NOTE]
  > During the lab test, the tables and data were verified through the canvas's own actions, without visually inspecting the rendered panel. That confirms the data checks, not the panel's visual rendering. Confirm the panel visually yourself before treating that part of the exercise as complete. Use the address and port reported by your session if needed; neither is fixed.

  ### Troubleshoot the Database Explorer

  #### Missing dependency

  If `.github/extensions/database-explorer/extension.mjs` fails to load with `Cannot find package '@libsql/client'`, run the following command from the current worktree's root:

  ```bash
  npm install --no-save @libsql/client
  ```

  > [!NOTE]
  > `npm ci` removes the dependency installed with `--no-save`. If you run `npm ci` again, reinstall `@libsql/client` before running `/restart-session`.

  #### Database path mismatch

  Check whether `DATABASE_URL` is defined for the canvas and compare its resolved database path with the project's scripts. In the tested setup, the scripts created `tailspin.db` at the worktree root, but the extension looked for `.data/tailspin.db` when `DATABASE_URL` was not set. Do not let the extension create or read a separate empty database by mistake.

  The workaround validated on Windows was to create `.data/tailspin.db` as a hardlink to the root `tailspin.db`, then add `/.data/` to Git's local exclude file, not the tracked `.gitignore`. A hardlink refers to the same database, not a copied database. This is a **local workaround**, not a permanent project fix, and requires a filesystem that supports hardlinks on the same volume.

  From the current worktree's root, use PowerShell to check the paths and create the link only if the source exists and the destination does not:

  ```powershell
  if (-not (Test-Path -LiteralPath 'tailspin.db' -PathType Leaf)) {
    throw 'The root tailspin.db is missing. Check the project setup output first.'
  }
  if (Get-Item -LiteralPath '.data/tailspin.db' -Force -ErrorAction SilentlyContinue) {
    throw 'The destination already exists. Inspect it; do not overwrite it.'
  }
  New-Item -ItemType Directory -Path '.data' -Force -ErrorAction Stop | Out-Null
  New-Item -ItemType HardLink -Path '.data/tailspin.db' -Target (Resolve-Path -LiteralPath 'tailspin.db').Path -ErrorAction Stop
  $excludePath = git rev-parse --git-path info/exclude
  if ($LASTEXITCODE -ne 0) {
    throw 'Could not locate the local Git exclude file.'
  }
  Add-Content -LiteralPath $excludePath -Value "`n/.data/"
  ```

  If the destination already exists, stop and inspect which database it contains; do not delete or replace it. If hardlinks are unavailable, stop and report the blocker instead of copying or overwriting a database. After resolving the dependency or path issue, run `/restart-session` in the same session and worktree, then ask Copilot to reopen the canvas and repeat the table, data, and visual checks.

## Create a canvas to triage issues

Now create a different kind of shared surface. Saving the triage canvas at project scope makes it a repository asset that the team can review and reuse.

1. In the same session, enter `/create-canvas`, then describe the canvas you want to create:

   ```plaintext
   Create a Kanban triage canvas for this repo's open issues and save it under .github/extensions/. Highlight the three issues you'd prioritize and explain why, with the rest below. Include summaries and links.

   Give each card an "Add to current context" action that adds the issue details without starting work or changing the issue. Make it keyboard-accessible and open it so I can try it.
   ```

Copilot creates the canvas extension under `.github/extensions` and opens the shared surface in the app's right side panel. The generated extension is executable repository content, not just a visual artifact, so you'll inspect its files and behavior next.

## Inspect and exercise the canvas

Before sharing the canvas, compare it with the repository's actual issues and exercise its controls. This confirms that its content is accurate, its interaction is accessible, and its issue action adds context without starting work.

1. Open **Changes** and confirm the canvas definition is repository-backed under `.github/extensions/`, not saved only for your user or session. Check that existing extensions and application files are unchanged.
2. Compare the board with the actual open issues and assess the ranking explanations.
3. Check that cards and controls are readable and usable with a keyboard.
4. Select **Add to current context** for an issue and confirm only its details enter the conversation. No implementation or issue-state change should start.
5. Review any corrections and ask Copilot to run the applicable existing validation for the files changed. Record results and blockers, rather than assuming an interactive surface is correct because it opened.
6. If the canvas needs changes, request focused improvements within the triage scope, then repeat the affected checks. Do not implement one of the backlog issues as part of this canvas work.

The workshop stops before creating another PR because you've already practiced both manual merging and Agent Merge. In production, review and merge the canvas through your team's normal process before others rely on it.

## Summary and next steps

You created and reused a shared surface where you and the agent can collaborate. In this lesson, you:

- understood what a canvas is and when to use one.
- used the existing Database Explorer canvas to inspect project data.
- created a shared Kanban board canvas to triage your backlog.
- inspected and exercised the new canvas without implementing another feature.

With your backlog tracked, you'll [review everything you've built and explore where to go next][next-lesson].

## Resources

- [Working with canvas extensions in the GitHub Copilot app][canvas-docs]
- [Canvases on Awesome Copilot][awesome-copilot-canvases]
- [About the GitHub Copilot app][about-copilot-app]

[next-lesson]: ../10-review/
[canvas-docs]: https://docs.github.com/copilot/how-tos/github-copilot-app/working-with-canvas-extensions
[awesome-copilot-canvases]: https://awesome-copilot.github.com/extensions/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
