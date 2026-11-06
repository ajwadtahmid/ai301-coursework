# Playbook: [Student name]

<!--
Your playbook: the rules the PR monitor drafts from. It decides how
you answer review, how many quiet days are normal for your repo, what
a polite nudge says, and when closing your own PR is the right move.
You write every word. Where your rule is stricter than the monitor's
move forms, your rule wins.

It lives at ~/.claude/pr-monitor/playbook.md (the install's third
command copies this template there once). Fill it in there. The
monitor won't draft while any section below is empty, because a nudge
with no threshold behind it would be the monitor's call, not yours.

Keep it under a page: a few rules you mean, each concrete enough to
decide a real case. The Upstream section of your voice guide, from
unit 6, already has rules for claims, questions, and pings, and its
pings rules are a good start for Nudge etiquette.

Keep the four section headings exactly as they are, because the
monitor finds them by name. Delete this comment when you're done.
-->

## Response rules

<!-- What your replies to a reviewer open with, and what they never
do. Make each rule concrete: "answer the reviewer's point in the first
sentence" decides a case, "be professional" doesn't. 1 to 3 rules. -->

## Staleness thresholds

<!-- How many quiet days are normal for your repo before you'd
nudge? Check CONTRIBUTING.md for a stated wait, then look at five
recent merged PRs from outside contributors (skip ones a maintainer or
a bot opened, since those merge fast) and how many days each waited
for a maintainer's first comment, review, or merge. Pick a number a
few days past the usual wait, and write it with the reason. A different number
for a first review and for a stalled review is fine. The monitor counts
quiet days as days since anyone but you commented. -->

## Nudge etiquette

<!-- Your rules for the one nudge your threshold allows: what it
says, what it never says, who it tags, and whether you'd ever send a
second one. Start from your unit 6 pings rules: someone reading only
this comment knows what the PR does, it asks for one thing, and it
tags only the reviewer. -->

## Walk-away criteria

<!-- When would closing your own PR be the right move (a maintainer
says no, the issue gets fixed another way, the repo stays quiet longer
than you'll wait)? And what would your close note say? Closing it
yourself with a short note is an honest ending, and your PR was graded
when you opened it. -->
