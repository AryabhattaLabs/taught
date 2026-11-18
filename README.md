# Taught (beta)

Correct one agent once. Every agent on every machine remembers.

Taught keeps what you teach your coding agents (Claude Code, Codex, Cursor, Antigravity / Gemini CLI) in a private git repo you own, and writes it into the files each agent already reads, on every device.

**New here? Read [Getting started](GETTING-STARTED.md)**: install, a 2-minute test, a second laptop, the UI and the librarian.

## Start

You need Node 20+, git, and an empty **private** GitHub repo for your memory (for example `you/taught-memory`). For work, it can be a private repo in your company's GitHub org, one per person.

```bash
npm i -g https://github.com/ajeetsk/taught-releases/releases/latest/download/taught.tgz
taught setup --remote git@github.com:you/taught-memory.git --repos ~/code
taught doctor
```

`taught setup` joins (or starts) your memory repo, finds your code folders, adds Claude Code hooks, starts a background sync (macOS) and syncs once. On a second laptop, run the same `setup` with the same memory repo.

Then work as usual. When you correct an agent, it records the rule with `taught note`, and it reaches your other agents and devices within minutes. Review and edit everything in `taught ui`.

## What Taught changes on your machine

- A block between `<!-- taught:begin … -->` markers in `~/.claude/CLAUDE.md`, `~/.codex/AGENTS.md` and `~/.gemini/GEMINI.md`. Nothing outside the markers is touched; a copy of each file as it was is kept in `~/.taught-local/backups/`.
- In a folder that holds a project's repos: the same kind of block in `CLAUDE.md` and `AGENTS.md`.
- Inside your repos: only files Taught creates (`AGENTS.md`, `CLAUDE.local.md`, `.cursor/rules/taught*.mdc`), each listed in that repo's `.git/info/exclude`, so they never show up in `git status` or a commit. A repo's own committed `AGENTS.md` is never touched.
- Two Claude Code hooks in `~/.claude/settings.json`, and a background job (`~/Library/LaunchAgents/dev.taught.sync.plist`).
- Your memory lives in `~/.taught` and your memory repo. Nothing is sent anywhere else. No telemetry.

## Update

Run the install command again: it always fetches the latest beta. `taught --version` shows which one you have.

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
