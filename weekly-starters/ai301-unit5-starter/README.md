# AI301 Unit 5 starter: issue-scout

Materials for Unit 5 of AI301 (contributing in the wild). This repo
holds the unit's runnable artifact: the issue-scout skeleton, installed
as an overlay onto your Unit 1 skill. All instructions live on the
course portal (Overview, Activity, and Check-In tabs for Unit 5); this
repo is the package those pages tell you to install.

## What's here

- `skill/`: three files. `SKILL.md` (staff-authored, complete) is the
  scout frame: it drives `gh search issues`, ranks the combined
  results with your filters, and grades the top candidates with YOUR
  rubric, exactly as Unit 1 did. `scope.md` ships filled with the
  real-world scope and a slot for your fit profile. `search-strategy.md`
  is a template with zero content: writing the strategy is the Unit 5
  judgment work.

## Install (an overlay, not a fresh skill)

The Activity tab has the full steps and the warnings. In short: open
your installed `~/.claude/skills/issue-select/scope.md` and copy your
fit profile out FIRST (the install replaces that file), then from this
clone:

    cp skill/SKILL.md skill/scope.md ~/.claude/skills/issue-select/
    cp skill/search-strategy.md ~/.claude/skills/issue-select/
    mv ~/.claude/skills/issue-select ~/.claude/skills/issue-scout

Yours, kept: `rubric.md`, `references/evidence-guide.md`, and the fit
profile you paste back into the new `scope.md`. If the install clobbers
any of them, restore them from your course repo (the Module 1
`tools/issue-select/` upload holds all three).

## Run a scan

With the strategy written and the `gh` CLI logged in:

    claude "issue-scout: run a scan"

Save every read-out: copy from the first summary line through the
closing JSON fence into `~/.claude/skills/issue-scout/scan-output.md`.
Direct grading still works too: `claude "issue-scout: grade <URL>"`.

## No eval/ here, on purpose

The wild has no gold labels, so this unit ships no eval set. The
regression instrument for your patched rubric is Unit 1's harness,
unchanged: run it from the `eval/` directory of your
[Unit 1 materials](https://github.com/smoran02/ai301-unit1-starter)
clone, as the Activity tab's after-class steps describe.
