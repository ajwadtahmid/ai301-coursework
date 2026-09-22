# AI301 Unit 1 starter: issue-select

Materials for Unit 1 of AI301 (issue selection). This repo holds the
week's runnable artifacts: the issue-selection skill and its eval
harness. All instructions live on the course portal (Overview,
Activity, and Check-In tabs for Unit 1); this repo is the package those
pages tell you to install and run.

## What's here

- `skill/`: the issue-selection skill for Claude Code, complete except
  for one file: `skill/rubric.md` ships as an empty template. Writing
  the rubric is the Unit 1 deliverable.
- `eval/`: the eval harness, the gold labels, and 24 frozen issue
  bundles (20 scored plus the 4 calibration issues from the in-class
  activity). See `eval/README.md` for the full run and output guide.

## Install the skill

Copy the whole `skill/` folder to `~/.claude/skills/issue-select/`
(create the folders if they do not exist). Edit your rubric inside that
installed copy and point the harness at the same file, so eval runs and
live runs share one canonical rubric.

## Run the eval

From `eval/`, with the Claude Code CLI installed:

    python3 run_eval.py --rubric path/to/your-rubric.md

`--limit 3` gives a smoke run. Full docs: `eval/README.md`.
