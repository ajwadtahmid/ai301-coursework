# Installing the PR monitor

The PR monitor reads your pull request on GitHub and drafts your next
move into a file. The install copies one folder into your home
directory, `~/.claude/pr-monitor/`:

- `monitor.py`, the program itself
- `move-forms.md`, the instructions it gives Claude for each kind of
  draft
- `playbook-template.md`, which becomes your `playbook.md`
- this file

Nothing else changes. Your other tools (the scout, repro-check,
plan-check, pr-precheck, the voice guard and your voice guide, the
ship-gate and your manifest), your Claude Code settings, and your
course repo stay exactly as they are.

There's no hook and no settings change, because the monitor only runs
when you run it, from any terminal, with no Claude Code session open.

What it can and can't do:

- It only reads. Every `gh` command it can run is on a short read-only
  list in `monitor.py`, and it refuses anything else.
- Drafts go into files on your computer, never onto GitHub.
- It never edits your branch. A rebase plan is a plan you run.
- It runs when you run it, never on a schedule.

You post every reply yourself, through Claude Code, where your unit 6
voice guard checks it (section 5).

## 1. The install: three commands

From your Unit 8 materials folder:

```
mkdir -p ~/.claude/pr-monitor
cp skill/monitor.py skill/move-forms.md skill/playbook-template.md skill/INSTALL.md ~/.claude/pr-monitor/
cp ~/.claude/pr-monitor/playbook-template.md ~/.claude/pr-monitor/playbook.md
```

Run the third command only once. It makes your `playbook.md` from the
template, so running it again would replace your rules with a blank
copy. The first two commands are always safe to run again: they
update the monitor's own files and never touch `playbook.md`.

## 2. Check that it can see your PR

From the clone of your fork, switch to your PR's branch (the monitor
finds your PR by the branch you're on), then run the monitor:

```
git switch <your branch>
python3 ~/.claude/pr-monitor/monitor.py --state
```

This prints your PR's state: its checks, the review decision, the
comments, how many days it's been quiet, and whether it can merge. Its
last lines say whether your playbook is ready for a full run, and warn
you if the PR on this branch isn't yours. It makes no Claude call and
drafts nothing, so it's free, and it works before your playbook is
written.

Some of the words in the state are GitHub's:

- **Review decision REVIEW_REQUIRED**: the repo wants a review before
  merging, and none has landed yet. That's normal for a new PR.
- **CHANGES_REQUESTED**: a reviewer asked for changes. GitHub keeps
  showing it until they review again, even after you've replied, so
  the monitor only treats it as new feedback until your reply is
  posted.
- **Mergeable CONFLICTING**: your branch no longer applies cleanly to
  the base branch. A full run drafts a rebase plan.
- **Merge state BLOCKED**: GitHub is waiting on something the repo
  requires, usually that review. It isn't a verdict on your work.
- **UNKNOWN**: GitHub hasn't worked out the answer yet. A later run
  filling it in isn't a change to your PR.

Once a PR is merged, its checks and mergeability lines are leftovers,
because GitHub stops updating them. And on the first run against a
PR, only comments newer than your newest commit count as new feedback
(or comments from the last 14 days, if commit dates aren't
available), so an old bot comment doesn't read as news.

**If it says `NO PR YET`:**

- You haven't opened your PR: that's expected. Open it first (the unit
  7 Assignment tab), then run this again.
- You've opened your PR (even if it's merged or closed now): read
  the line that names the repository. The
  monitor looks for your PR there. If it names your fork, run
  `gh repo set-default <owner>/<repo>` with the original repo's owner
  and name (unit 7, step 11). When it works, it prints a line
  starting with ✓ that names the repo. Then run again. If the repository is
  right, check that you're on your PR's branch, or run
  `gh pr checkout <number>` to switch to it.

## 3. A full run

Fill in your playbook first (`~/.claude/pr-monitor/playbook.md`, four
sections, under a page). The monitor won't draft while a section is
empty. Then, from your clone, on your PR's branch:

```
python3 ~/.claude/pr-monitor/monitor.py
```

A full run reads the state, picks the move form that fits it, and
makes one Claude call with your playbook, the form, and your PR's
thread. It writes two files beside the monitor: `run-report.md` (your
PR's state) and `drafted-move.md` (the draft). Every run that reads
your PR also adds a dated block to `monitor-log.md`. A run costs a few cents, up to about
$0.25 on a long thread, and takes a minute or two.

Two answers are free, because there's nothing to draft:

- **NO PR YET**: see section 2.
- **NOTHING NEW**: your PR and your playbook are the same as at your
  last full run on this PR, so waiting is the move. It leaves
  `drafted-move.md` alone, so the draft you were working on is still
  there. When your PR changes, or you change your playbook, the next
  run drafts again.

The monitor remembers each PR separately, so a run on one PR never
changes how another one is read.

## 4. When the monitor stops

Every stop says why and what to do. Nothing is ever posted or half
done.

- **The PR monitor is not installed.** You ran `monitor.py` from your
  Unit 8 materials folder. Run the installed copy,
  `~/.claude/pr-monitor/monitor.py`, after section 1.
- **Not inside a git clone.** `cd` into the clone of your fork and
  switch to your PR's branch, because the branch is how it finds your
  PR.
- **`NO PR YET` for a PR you've opened.** gh is looking in the
  wrong repository, usually your fork. Section 2 has the fix.
- **The PR on this branch was opened by someone else.** You're on the
  wrong branch, often `main`, which can match a stranger's PR. Switch
  to your PR's branch (`git switch <your branch>`) and run again.
  `--state` shows the same thing as a warning.
- **gh is not logged in, or is rate-limited.** The stop quotes what gh
  said. Run `gh auth status` to see it, and `gh auth login` to log in.
  A rate limit clears on its own.
- **REFUSAL.** A playbook section is still empty, and the message
  names it. The monitor drafts from your rules, so it won't draft
  without them. Fill in that section and run again. `--state` works
  in the meantime.
- **Some other gh error.** The stop quotes gh's own words. Fix what
  they name and run again. The monitor stops rather than guess at your
  PR's state.
- **The Claude call failed or timed out.** Nothing was drafted or
  posted. Run again, and ask in your course's build-support Slack
  channel if it keeps happening. `--state` still works.

## 5. The files it keeps, and posting a draft

`~/.claude/pr-monitor/monitor-log.md` keeps one dated block per run
and only grows. `run-report.md` and `drafted-move.md` hold the latest
run. They're replaced on every run that drafts, and NOTHING NEW leaves
them alone. All of it stays on your computer. Assignment 5 doesn't
ask for any of it, and your unit 9 portfolio can quote it.

Never post `drafted-move.md` itself. It has a DRAFT banner, a MOVE
line, and notes in brackets written to you, and none of that belongs
on a maintainer's thread. To post a draft:

1. Copy the draft's text into a new file, `my-reply.md`, in the top
   folder of your clone, and rewrite it until every word is yours. Run
   `echo my-reply.md >> .git/info/exclude` once, so the file stays out
   of your commits.
2. From your clone, post that file through Claude Code:

```
claude "run this command: gh pr comment <PR URL> --body-file my-reply.md"
```

Keep the file in your clone and the path short, because the voice
guard reads the file from the folder the command runs in and can't
follow a `~/...` path. Your unit 6 voice guard checks it against your
voice guide before it posts. If the guard stops it, fix the line it
quotes and run the command again. Posting from the browser instead?
Check the file with the guard first, the way unit 6 showed you:

```
python3 ~/.claude/voice-guard/check_voice.py --draft my-reply.md
```

## 6. Running a rebase plan

A rebase plan is for you to run, on your branch. Nothing gets posted.
The plan names the exact commands for your PR, and they usually look
like this:

```
git remote add upstream https://github.com/<owner>/<repo>.git
git fetch upstream
git rebase upstream/<base branch>
git push --force-with-lease origin <your branch>
```

- Run the first command only if `git fetch upstream` fails. Your
  clone's `origin` is your fork, and `upstream` is the original repo.
- Where git stops on a conflict, run `git status` to see which files
  conflict. In each one, git marks the clash with `<<<<<<<`, `=======`,
  and `>>>>>>>` lines. Keep the lines that belong (often some of both
  sides), delete the three marker lines, then run `git add <file>` and
  `git rebase --continue`. If it goes wrong, `git rebase --abort` puts
  your branch back the way it was.
- Before you push, run each command in your `ship-manifest.md`'s
  first table again.
- The rebase rewrote your branch, so a plain `git push` is rejected.
  `--force-with-lease` replaces your fork's copy of the branch, and
  refuses if someone else pushed to it since you fetched.
