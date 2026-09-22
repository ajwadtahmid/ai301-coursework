# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Repo is maintained | Repo facts: archived status, last push date, latest release date | Not archived AND (last push within 6 months OR latest release within 1 year) | required |
| Maintainer is present | Repo facts: last 5 default-branch commits | Most recent commit is within 3 months, showing active maintenance | required |
| Not claimed | Repo facts: assignees, linked PRs for this issue | No assignee set AND no open linked PRs | required |
| Scope is bounded | Issue text, title, and linked PRs | Not labeled as megaissue/epic/tracking; has clear reproduction steps or stated requirements; does not have multiple abandoned/closed linked PRs showing scope creep | required |
| No AI policy barrier | Repo facts: contribution policy | Policy does not forbid AI-generated code or documentation | required |
| Has first-issue label | Issue labels | Has "good first issue" or similar label | preferred |

## Verdict rule

Accept if all five required checks pass. If any required check fails, reject. Preferred checks do not affect verdict. Unclear on required checks counts as fail.
