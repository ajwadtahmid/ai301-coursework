# AI301 Unit 8 materials: the PR monitor

The PR monitor is a program you run from your terminal. It reads your
pull request on GitHub (its checks, reviews, comments, and how long
it's been quiet), then asks Claude to draft your next move into a
file, using a playbook of rules you write. It never posts anything.

The steps are on the Unit 8 **Assignment** tab in the course portal.
This repo holds the files those steps use.

## Get it

```
git clone https://github.com/codepath/ai301-unit8-starter.git
```

Or click **Code**, then **Download ZIP**, on this page. Then follow the
Assignment tab, starting with the install, from this folder:

```
mkdir -p ~/.claude/pr-monitor
cp skill/monitor.py skill/move-forms.md skill/playbook-template.md skill/INSTALL.md ~/.claude/pr-monitor/
cp ~/.claude/pr-monitor/playbook-template.md ~/.claude/pr-monitor/playbook.md
```

Run the last command only once, because it replaces `playbook.md` with
a blank copy. You need Python 3.8 or newer (`python3 --version`) and
the GitHub CLI, logged in (`gh auth status`).

## What's here

- `skill/monitor.py`: the PR monitor. From your clone, on your PR's
  branch, `python3 ~/.claude/pr-monitor/monitor.py` drafts your next
  move into `drafted-move.md`, and `--state` prints your PR's state for
  free, with no Claude call.
- `skill/playbook-template.md`: the starting point for your
  `playbook.md`, with four sections for you to fill in: response rules,
  staleness thresholds, nudge etiquette, and walk-away criteria.
- `skill/move-forms.md`: the instructions the monitor gives Claude for
  each kind of draft (a reply to review, a rebase plan, a nudge, a
  merged follow-up). Your playbook's rules win wherever they're
  stricter.
- `skill/INSTALL.md`: what the install does, how to check that the
  monitor can see your PR, how a full run works, what to do when it
  stops, how to post a draft, and how to run a rebase plan.

You write no code this unit. What you write is your playbook.

There's no hook this unit, because the monitor only runs when you run
it, and there's no eval set, because it only reads real PRs.
