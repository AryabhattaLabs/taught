# Taught (beta)

Correct one agent once. Every agent on every machine remembers.

Taught keeps what you teach your coding agents (Claude Code, Codex, Cursor, Antigravity / Gemini CLI) in a private git repo you own, and writes it into the files each agent already reads, on every device.

It keeps rules, not conversations. A rule is something you want followed from now on: a correction, a way you like work done, a project's convention. What an agent said and read in a session (its context and chat history) stays with that agent.

**New here? Read [Getting started](GETTING-STARTED.md)**: install, a 2-minute test, a second laptop, the UI and the librarian.

**Questions?** The [FAQ](FAQ.md) covers how this differs from your own git repo and a cron job, what protects a team's rules, and what leaves your machine.

## Is it useful with one agent on one laptop?

Yes. Several agents and laptops is where Taught helps most, but with one agent you still get:

- **Rules that follow you across projects.** A correction about how you work (how you like PRs, tests or commit messages) becomes a personal rule. It applies in every project, not only the one where you said it.
- **Your other open sessions hear about it.** With Claude Code, a rule recorded in one session reaches your other open sessions at their next prompt.
- **One page for everything your agent has learned.** `taught ui` shows every rule, by project, for you to edit or delete. It also brings in what Claude Code has already saved.
- **A backup with history.** Your memory is in your own private git repo. A new laptop, or a second agent later, starts fully taught.

## For a team

A team can share one project's memory through a private git repo that all of you can write to, while each person keeps their own personal rules. Nothing of yours is published into it without your yes, and removing the project on your laptop doesn't take it from the others. See [Sharing a project's memory with your team](GETTING-STARTED.md#sharing-a-projects-memory-with-your-team). Everyone needs 0.1.0-beta.9 or later.

## Start

You need Node 20+, git, and an empty **private** GitHub repo for your memory (for example `you/taught-memory`). For work, it can be a private repo in your company's GitHub org, one per person.

```bash
npm i -g https://github.com/ajeetsk/taught-releases/releases/latest/download/taught.tgz
taught setup --remote git@github.com:you/taught-memory.git
taught doctor
```

`taught setup` joins (or starts) your memory repo, finds your code folders, adds Claude Code hooks, starts a background sync (macOS) and syncs once. It registers nothing by itself: it ends on the **Projects** page of `taught ui`, where you choose which code Taught keeps memory for and where that memory is kept (your memory repo, this laptop only, a separate git repo, or Off). On a second laptop, run the same `setup` with the same memory repo.

Then work as usual. When you correct an agent, it records the rule with `taught note`, and it reaches your other agents and devices within minutes. Review and edit everything in `taught ui`.

## What Taught changes on your machine

- A block between `<!-- taught:begin … -->` markers in `~/.claude/CLAUDE.md`, `~/.codex/AGENTS.md` and `~/.gemini/GEMINI.md`. Nothing outside the markers is touched; a copy of each file as it was is kept in `~/.taught-local/backups/`.
- In a folder that holds a project's repos: the same kind of block in `CLAUDE.md` and `AGENTS.md`.
- Inside your repos: only files Taught creates (`AGENTS.md`, `CLAUDE.local.md`, `.cursor/rules/taught*.mdc`), each listed in that repo's `.git/info/exclude`, so they never show up in `git status` or a commit. A repo's own committed `AGENTS.md` is never touched.
- Three Claude Code hooks in `~/.claude/settings.json` (sync when a session starts, sync when Claude saves a memory, and tell an open session about memory that changed), and a background job (`~/Library/LaunchAgents/dev.taught.sync.plist`).
- Your memory lives in `~/.taught` and your memory repo, plus anything you keep on this laptop only or in a separate location you attach (`~/.taught-local`). Nothing about you, your code or your memory is sent anywhere else. No telemetry.
- Once a day, Taught asks GitHub whether a newer version exists, by fetching one small public file from this repo's releases. Nothing from your machine is sent with that request. GitHub's download count for that file is the only usage figure Taught's author gets: roughly how many devices run it. Turn it off on a device with `taught update-check off`.
- In a repo you set to **Off**, Taught writes nothing at all.

## Update

Run the install command again: it always fetches the latest beta. `taught --version` shows which one you have, and `taught doctor`, `taught status` and a sync you run yourself say when a newer one exists.

```bash
npm i -g https://github.com/ajeetsk/taught-releases/releases/latest/download/taught.tgz
```

## Undo

```bash
taught uninstall          # says what it will remove
taught uninstall --yes    # removes it; your memory repo is kept
npm uninstall -g taught
```

## Feedback

Post on the [issues page](https://github.com/ajeetsk/taught-releases/issues/new/choose). `taught feedback` copies a setup report (versions and checks, never your memory) to paste into the form. Issues are public: describe your rules and projects generically.

This is an invite-only beta: you may share it with people on your own team, but not beyond. See TERMS.md.
