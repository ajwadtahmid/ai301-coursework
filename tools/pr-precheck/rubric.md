# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Plan fidelity | Diff against plan's Files and Approach sections; description claims against diff's actual behavior | Diff stays within plan boundary: all changed files appear in plan's stated Files section, OR plan explicitly notes a deviation for each file outside scope. When diff contains less than plan (fewer files, incomplete steps), the description and plan both acknowledge it. Description's claims of what the PR does match diff's actual content (no claiming more or less than delivered, no silent scope creep). | required |
| Test evidence | Test evidence section against plan's test plan; repo's own checks; before/after observable | Plan's test plan scenario(s) are re-run and before/after shown concretely: behavior named, expected-after stated, output or screenshot visible. Repo's own checks (pytest, cargo test, CI suite) run and pass. If plan names multiple failure modes, each is tested; if one scenario is tested, the scenario matches plan's test plan. | required |
| Diff quality | Unified diff; commit messages | The change is visible and complete: no debug prints, commented-out code, dead functions, TODOs, or reformatting churn bury the fix. Commit messages are clear (not 'wip', 'fix', 'fmt'). One logical change per PR, or multiple bundled changes explicitly named in commits and description. | required |
| Standards and disclosure | Repo facts' PR template sections; stated AI policy; PR description text | All required template sections (checklist items, type-of-changes, changelog entries, etc.) filled with real content, not boilerplate or skipped. If repo's stated policy requires AI-use disclosure, description contains clear disclosure; absence of required disclosure fails the check. | required |
| Description fidelity | Description text against diff | Description's scope and claims match diff's scope and content. A description claiming "closes #X" must have diff touching issue X's behavior. A description claiming "no other changes" must have no unrelated edits. A description claiming a complete fix without noting a deferred part fails when plan deferred that part. Scope match is exact. | required |
| Honest deferral | Plan's scope section and deviation notes; description | When plan defers a stated part of the issue ("percent encoding left for follow-up", "lazy loading out of scope"), a PR delivering the deferred part's issue with an honest note passes. A PR silently delivering less than full issue without restating plan's deferral fails; a PR restating the deferral passes. | preferred |

## Verdict rule

Accept if every required check passes; preferred checks never change the verdict. Unclear counts as fail on required checks (if evidence is absent or rule is too vague to apply, the check fails). First required check to fail in rubric order is the deciding check quoted in the verdict.
