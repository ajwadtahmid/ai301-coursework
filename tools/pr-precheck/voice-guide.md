# Voice guide: how I talk upstream

<!--
THIS IS A CARRY-OVER SLOT, not a new hole. You wrote this guide in
week 2 and carried it through week 3; paste your filled voice-guide.md
here, whole. It is not re-authored and it is not graded as new work
this week.

Then reread it with the PR in mind. Your comments promised, reported,
and committed to an approach; a PR title and description ask a
maintainer to spend review time on your work. If your rules do not
cover that register (for example: how a title earns its thirty
seconds, how a description promises exactly what the diff contains,
how a disclosed shortfall is worded so it reads as honesty rather
than apology), extend the guide with what it needs. Extending is
allowed and encouraged; starting over is not required.

Live mode reads this file and holds your draft PR title and
description against it, reporting any rule your draft breaks. Eval
mode ignores it entirely, because your voice is yours and carries no
gold labels.
-->

## Who I am in threads

I'm contributing to open source as a student in an AI course, working through issue diagnosis, planning, and implementation. My PR descriptions reflect the specific plan I posted, the work I did to implement it, and honest accounting of what I'm delivering and what I'm deferring. Readers should expect accurate scope claims, concrete before/after evidence, and clear ownership of my choices.

## Rules I write by

### Rule: Description scope must match diff scope exactly

The description's promised scope (what files, what behavior) must match the diff's actual files and changes. No unannounced extras, no unmentioned deferrals.

- Wrong: "Implements the plan exactly" (description claims) while diff adds a new feature and a refactoring the plan didn't mention
- Right: "The fix for symlinks falls through to logical-path display. The refactor is a prep for the next fix on a separate issue; see issue #XXXX."

### Rule: Before/after evidence must name the observable behavior

When the plan's test scenario is shown, name what the issue's repro step produces and what the fix delivers. "Tests pass" alone is not decisive evidence.

- Wrong: "Verified working: repo tests pass"
- Right: "Issue repro: `starship prompt` at the symlinked path rendered no directory; after the fix, it shows `~/projects/app-dir` (logical path, not repo-root style). `cargo test directory` passes all 12 tests."

### Rule: Defer explicitly, don't silently drop plan content

If the plan promises both X and Y and you're delivering only X, say so. Use the plan's own language for what you're deferring.

- Wrong: "This implements the fix" (silently omitting the promised documentation update)
- Right: "This implements the token reservation fix. The lazy-loading optimization is left for follow-up on issue #XXXX per the plan's note."

### Rule: Honor repo's stated template and disclosure policy

Fill every template section the repo requires. If the repo requires AI-use disclosure, include a clear statement.

- Wrong: "Closes #XXXX" field left empty; PR description has no changelog entry when the repo template asks for it
- Right: "Closes #66657. Changelog entry added to doc/source/whatsnew/v3.0.6.rst. AI disclosure: this fix was developed with Claude AI."

## Things I never post

- Description claims contradicted by the diff ("no other changes" when unrelated edits are present, "comprehensive fix" when deferred parts exist)
- Before/after evidence absent or exercising only the unchanged path (not the issue's actual repro)
- Template sections ignored or filled with boilerplate; required fields left blank
- Promised AI disclosure missing when the repo's policy requires it
- Unacknowledged scope creep ("same approach as the plan" when the diff does more than planned)
