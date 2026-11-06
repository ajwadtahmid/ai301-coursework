# This week's rubric patch: scope informs instead of rejecting

<!-- Staff-shipped patch text for YOUR rubric.md. Paste the row and
the section below into your rubric (the row replaces whichever check
in your table grades scope, from the unit-1 family "the scope fits a
newcomer"; the section goes under your checks table), reword them in
your own voice if you like (it is your rubric), and add a dated
line to scan-notes.md naming the check you replaced. Delete this comment block wherever
you paste. -->

## Why this patch exists

Unit 1's sandbox was seeded with perfectly bounded issues, so a strict
scope check cost you nothing there. The wild almost never ships a
perfectly bounded issue: a real, claimable piece of work usually
carries an open edge (an "etc.", an undecided design choice, a file
nobody named). A check that rejects every open edge rejects most of
the wild's honest work, and it is why your scout can grade ten real
issues and accept none. So scope stops deciding accept or reject and
becomes a difficulty note: the floor only rejects issues that are not one piece
of work at all, and everything above the floor gets flagged, not
filtered. You choose with eyes open; an open edge in an issue you
claim becomes your first question to the maintainer, which is
exactly the claim craft this week teaches.

## The replacement row (replaces your scope check in your checks table)

You wrote your own rubric in unit 1, so your checks carry your own
names and there is no one row name to look for here. Replace the row
that grades whether the issue is a single bounded piece of work a
newcomer could take on, whatever you called it (the unit-1 lecture
called that family "the scope fits a newcomer"). If your table has no
scope row at all, add the row below as a new required check. Nothing
else in your table changes, and the row you swap out is worth naming
in your dated patch note.

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| scope-feasible | Issue body: the work it asks for | The issue describes one identifiable piece of work someone could start: a behavior, a gap, or a task you can point at. It fails only when there is no single piece of work in it: a codebase-wide checklist, an "improve X everywhere", or a container issue tracking many sub-tasks. An open design edge, an "etc.", or several files involved never fails this check; that raises the difficulty note instead. | required |

## The difficulty note (add this section under your checks table)

With each accepted candidate, report a one-line difficulty note in
the read-out summary:

- **straightforward**: the issue names where the change lives and the
  change is bounded.
- **moderate**: the location is yours to find, or several files are
  involved.
- **ambitious**: an open design edge, or an unfamiliar subsystem;
  name the edge, because claiming this issue means raising it with
  the maintainer early.

The note is information for the chooser, never a verdict input: it
does not appear in the fenced JSON block, it never changes accept or
reject, and in eval mode it is skipped entirely.

<!-- Honestly stated: this patch keeps true umbrella issues rejecting
(the floor catches them) while accepting bounded issues with open
edges. Note the patch and what drove it in the Toolkit block of your Why
I Chose This Issue section; the
patch itself is unit-9 portfolio material. -->
