# Taught: questions people ask

Short answers, including what Taught doesn't do yet. For setup, see [Getting started](GETTING-STARTED.md).

- [Why not my own git repo and a cron job?](#why-not-my-own-git-repo-and-a-cron-job)
- [Why not commit `CLAUDE.md` or `AGENTS.md` to the code repo?](#why-not-commit-claudemd-or-agentsmd-to-the-code-repo)
- [Doesn't each agent have its own memory?](#doesnt-each-agent-have-its-own-memory)
- [Can a teammate, or a confused agent, poison our team's rules?](#can-a-teammate-or-a-confused-agent-poison-our-teams-rules)
- [Where is my memory, and who can read it?](#where-is-my-memory-and-who-can-read-it)
- [What does Taught's author see?](#what-does-taughts-author-see)
- [Does Taught send my code or my rules to a model?](#does-taught-send-my-code-or-my-rules-to-a-model)
- [What if a rule contains a secret?](#what-if-a-rule-contains-a-secret)
- [What does it change on my machine and in my repos?](#what-does-it-change-on-my-machine-and-in-my-repos)
- [What if two people record a rule at once, or I'm offline?](#what-if-two-people-record-a-rule-at-once-or-im-offline)
- [Won't hundreds of rules crowd the agent's context?](#wont-hundreds-of-rules-crowd-the-agents-context)
- [Which agents, systems and git hosts work?](#which-agents-systems-and-git-hosts-work)
- [How do I stop using it?](#how-do-i-stop-using-it)
- [What does it cost?](#what-does-it-cost)

## Why not my own git repo and a cron job?

Underneath, that is what Taught is: a private git repo you own, and a sync every few minutes. If you use one agent on one laptop and write your rules down yourself, your own script will do.

What Taught adds is the work around the sync:

- **Capture.** The agent records the rule itself, with the reason, at the moment you correct it. Taught also brings in what Claude Code has already saved. Nobody has to remember to write anything down.
- **One rule, every agent's file.** The same rule is written into the file each agent reads (`CLAUDE.md`, `AGENTS.md`, Cursor rules, `GEMINI.md`), in every checkout and worktree of the right repos.
- **Levels.** A personal rule follows you into every project. A project rule covers all of that project's repos. A repo rule stays in one repo. A personal rule that names something from a client's project is held for your review first.
- **Clean code repos.** Taught changes only its own marked block, or files it created and told git to ignore. `git status` stays clean, and a file you wrote is never edited.
- **The same project in different folders.** A project is recognised by its repos' git addresses, so each laptop and each teammate can lay out folders their own way.
- **Sessions that are already open.** A Claude Code session gets a new rule with your next message.
- **A team.** A project's memory can be shared without publishing anyone's private notes, and removing it on your laptop doesn't remove it for the others.
- **Upkeep.** A size limit so rules don't crowd the agent's context, an optional librarian that suggests merges, a page to read and edit everything (`taught ui`), and a health check (`taught doctor`).

Your memory is plain markdown, one file per rule, in your own repo. You can stop using Taught and keep your script.

## Why not commit `CLAUDE.md` or `AGENTS.md` to the code repo?

Keep doing that for conventions the team has agreed on and wants reviewed like code. Taught never touches a committed `AGENTS.md` or `CLAUDE.md`.

Taught is for what doesn't fit there:

- **Personal rules.** How you like PRs, tests and commit messages belongs to you, not to one repo.
- **Corrections made in the middle of a task.** Recording one shouldn't need a commit and a pull request.
- **Projects made of several repos.** One rule reaches all of them.
- **Several agents.** Each reads a different file.
- **Memory that must not be in the code repo,** such as a client's.

## Doesn't each agent have its own memory?

Yes, and it stays with that agent on that machine. What Claude Code saves doesn't reach Codex or Cursor, or Claude Code on your other laptop.

Taught gives every agent the same rules on every device, and imports what Claude Code has already saved for the projects you choose.

## Can a teammate, or a confused agent, poison our team's rules?

A rule in a team's memory is read as an instruction by every teammate's agents, so treat the team's memory repo the way you treat code.

What protects you today:

- **Access is the repo's access.** The team's memory is a private git repo. Only people who can push to it can add or change a rule. Give that to the people you would let merge code.
- **Every rule says who recorded it,** with which agent and when. The name is the one git reports; your git host's own record of pushes is the certain one.
- **Everything is in the history.** Deleting a rule in `taught ui`, or reverting the commit, takes it away from everyone at their next sync.
- **Nothing of yours is published by itself.** Your saved Claude Code notes wait for your yes before they are shared. A project or repo a teammate adds isn't used on your laptop until you accept it.
- **A rule stays inside Taught's block.** Its text can't write into the rest of your `CLAUDE.md` or `AGENTS.md` (from 0.1.0-beta.12).
- **A rule is text, not permission.** It doesn't change what your agent is allowed to do: its permission prompts and sandbox stay as you set them.

What isn't there yet:

- **No review step per rule.** A rule a teammate's agent records reaches the others' agents at their next sync, without anyone approving it.
- **A team repo whose main branch is protected isn't supported.** Taught pushes straight to it.

Until review exists, a shared location suits a small team whose members trust each other's judgement. Review before a rule reaches the team is what we are building next.

## Where is my memory, and who can read it?

- On your laptop, in `~/.taught` and `~/.taught-local`.
- In the private git repo you gave Taught. Whoever has access to that repo can read it. Nobody else can.
- A project you share with a team is in the team's repo, readable by everyone with access to it.

There is no Taught server and no account.

## What does Taught's author see?

Nothing about you, your code or your memory. There is no telemetry.

Once a day Taught asks GitHub whether a newer version exists, by fetching one small public file. Nothing from your machine is sent with that request. GitHub counts how often that file is downloaded, and that count is the only usage figure there is. `taught update-check off` stops it on a laptop.

## Does Taught send my code or my rules to a model?

No. Taught has no model of its own.

The one exception is the librarian, which is optional and runs only when you start it:

- It uses an agent or key you chose, signed in as you.
- It reads only the projects you allowed. The default is none.
- It sees rule text, titles and the project's name. It never sees code.
- Emails, phone numbers, keys and passwords inside URLs are replaced with placeholders before anything is sent.

## What if a rule contains a secret?

Taught doesn't scan rules for secrets. A rule is stored as written.

Don't put keys in rules. If one gets in, delete the rule and replace the key: the repo's history still holds the old text.

## What does it change on my machine and in my repos?

The full list is in the [README](README.md#what-taught-changes-on-your-machine). In short:

- a marked block in each agent's own file, and in the folder that holds a project's repos;
- inside a repo, only files Taught created, which git is told to ignore;
- three Claude Code hooks and a background sync.

`taught uninstall` takes all of it out again.

## What if two people record a rule at once, or I'm offline?

- **At once:** each rule is its own file, so two new rules never clash.
- **The same rule changed by two teammates:** the team's version stays, yours is kept in the trash on your laptop, and the sync tells you.
- **Offline:** a rule is saved on your laptop and reaches your agents there straight away. It is pushed at the next sync that can reach the repo.

## Won't hundreds of rules crowd the agent's context?

Each agent file has a limit: about 150 lines of personal rules and about 220 for a project.

- Past the limit, rules are shortened to their title and the rule itself. The full text, with the reason, goes into a file on your laptop that the agent is pointed to.
- The librarian suggests merging duplicates and retiring one-off facts.
- Status notes are kept out of agent files altogether.

## Which agents, systems and git hosts work?

- **Agents:** Claude Code, Codex, Cursor and Antigravity. Gemini CLI is untested. Any other agent that reads `AGENTS.md` gets project memory. Details are in [Your agents](GETTING-STARTED.md#your-agents).
- **Systems:** macOS is what it is built and tested on. Linux is expected to work but is little tested, and there the background sync is a line you add to cron yourself. Windows is untested.
- **Git hosts:** Taught uses plain git, so any host you can clone from should work for the memory repo. GitHub is the only one it has been run against. Code repos on other hosts are recognised by their address.

## How do I stop using it?

```bash
taught uninstall          # says what it will remove
taught uninstall --yes    # removes it
npm uninstall -g taught
```

Your memory repo is kept. It is plain markdown, one file per rule, readable without Taught.

## What does it cost?

The beta is free, and by invitation. What comes after the beta isn't decided yet. Either way, your memory stays in your own repo as plain files.
