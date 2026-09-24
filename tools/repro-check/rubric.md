# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Environment recorded | Repro report's first section (Environment section from evidence-guide.md) | Names the tool version tested, the OS/platform, and any major dependencies (Python version, library versions mentioned in the issue's thread); version deltas are acceptable if stated; version numbers need not match the issue's stated target if the difference is acknowledged. | required |
| Steps followable | Repro report's Steps section (from evidence-guide.md) | Starting state is clear (or implied by the issue); exact commands or actions are given that trigger the reported behavior; critical flags or setup conditions match the issue's description; steps identify which output shows the failure. | required |
| Behavior matches issue | Repro report's artifacts (output, logs, error messages) read against the Issue description | Artifacts show the exact symptoms the issue names (not adjacent behaviors, generalized errors, or successful runs); an honest cannot-reproduce with environment detail and hypothesis counts as a match; over-claiming (presenting adjacent symptoms as the reported issue) fails. | required |
| Honesty | Repro report's summary + claim comment against the artifacts shown | Statements match artifacts: "crash destroys data" backed only by graceful error fails; "verified race condition" with no concurrent-execution evidence fails; pure me-too without reproduction intent ("+1") fails; over-promising ("guaranteed fix in 2 days") fails. Acceptable: "I will investigate", "I could not reproduce but here's why", honest assessment of environment differences. | required |
| AI disclosure if required | Claim comment + repo's stated "contribution policy" from Repo facts | If the repo's policy says "disclose all AI usage" or "disclose if AI-assisted", the claim comment must disclose that AI assistance was used; if the policy is silent or permissive, disclosure is not required; if disclosure is required and absent, the check fails. | required |

## Verdict rule

Accept if every required check passes; unclear on any required check counts as fail (the evidence to verify the check is absent, not just hard to evaluate).
