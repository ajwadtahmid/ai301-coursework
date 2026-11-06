<!--
THE REPO-MEMORY TEMPLATE (the unit-6 second hole).

Copy this file to the root of YOUR CLONE of the target repo as
CLAUDE.local.md, then fill it. CLAUDE.local.md is the documented
personal memory file: Claude Code loads it into every session you run
in that repo, alongside any CLAUDE.md the repo itself ships, and it
is never checked in. It is how expensive orientation gets paid once
and read back free all term.

STEP 0, before you write anything: check what the repo already ships.

  - A root CLAUDE.md: the maintainers' own conventions doc. Claude
    Code loads it next to this file automatically. Read it; do NOT
    duplicate what it already says here.
  - A root AGENTS.md: also the maintainers' own conventions doc, but
    Claude Code does not read AGENTS.md natively. KEEP the @AGENTS.md
    import line below as this file's first content line; it pulls the
    maintainers' doc into your context.
  - Both: the CLAUDE.md already loads automatically and the two are
    usually near-twins; delete the import line rather than loading
    the same conventions twice.
  - Neither: delete the import line and carry on.

  Either way, read the repo's CONTRIBUTING.md end to end first; it
  is the source for the conventions section, and you are held to it
  whether you read it or not.

THE SIZE DISCIPLINE. This file is loaded into every session's
context in this repo, so it is a cache of DISTILLED CONCLUSIONS, not
a filing cabinet: aim for under ~60 lines. That aim counts your
filled content: DELETE THIS WHOLE INSTRUCTION COMMENT once you are
done filling (it is scaffolding, not memory, and comments still cost
context). A few lines over with real conclusions is fine; a pasted
orientation transcript is not. Distill it, then delete it.

THE SCOPED ORIENTATION, once, after reading CONTRIBUTING.md: run one
agent over ONLY the subsystem your issue touches (the directory, its
key files, how they connect, where the tests live). Never whole-repo
mapping: you are fixing one issue, not writing the repo a guidebook,
and orientation you do not need is money spent on nothing. Distill
what it finds into the subsystem section below. A prompt that works,
from the clone root:

  claude "Read only <the folder your issue touches>. Tell me where
  <the behavior in my issue> lives, the 2-4 files that matter, how
  they connect, and where their tests are."

KEEP IT OUT OF YOUR PR DIFF. You are a guest: you cannot add ignore
rules to upstream's .gitignore. The local mechanism is
.git/info/exclude (works like .gitignore, never leaves your clone).
From the clone root:

  echo CLAUDE.local.md >> .git/info/exclude

Then confirm with `git status`: the file should not appear. The file
stays local to the clone forever; Assignment 5 collects no copy of
it, and the repo memory's story belongs in your contribution
story's Toolkit entry.
-->

@AGENTS.md

<!-- ^ Keep this line only if the repo ships a root AGENTS.md
(step 0). Delete it otherwise. -->

# My notes on [repo] (local, never committed)

<!-- One provenance line: repo, the issue you claimed, the date you
wrote this. -->

## Conventions from CONTRIBUTING.md

<!-- Only the rules that will actually steer your work: what a PR needs
before it's accepted, scope rules, branch and commit conventions, the process asks you
could get wrong. Quote thresholds exactly; paraphrase the rest. -->

## The subsystem my issue touches

<!-- The distilled conclusions of your one scoped orientation run:
where the behavior lives, the 2-4 files that matter, how they
connect, where the tests for them live. Conclusions only; the
transcript dies here. -->

## Build and test commands

<!-- The exact commands, copied working, not from memory. If the
repo's CLAUDE.md or AGENTS.md already states them, point there
instead of duplicating. -->

## House style and stated AI policy

<!-- The repo's stated style rules that apply to your change, and
its AI policy verbatim if it has one: course work is AI-assisted by
design, so any stated disclosure rule applies to you. No stated
policy? Write "none stated" and keep the course practice anyway:
disclose honestly, in your own words, in what you post. -->
