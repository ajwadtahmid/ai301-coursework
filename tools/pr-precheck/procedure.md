# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

1. **Plan context** (read first): From the plan-context section, extract and note:
   - Files: the exact list of files the plan names
   - Scope: the plan's stated boundaries and what is NOT in scope
   - Approach: the concrete steps the plan describes
   - Test plan: what behavior will be observed, what the test will re-run
   - Deviation notes: any explicit notes in the plan about what the PR will deliver less than full issue asks (e.g., "percent encoding left for follow-up")
   
2. **Diff** (read second, directly compare against plan):
   - List all changed files in the diff
   - Note any files NOT in the plan's Files list (potential scope creep)
   - Scan for debug prints, commented-out code, dead functions, formatting-only changes
   - Note commit messages (are they clear or generic?)
   
3. **Repo facts** (read third): Extract:
   - Required PR template sections and checklist items
   - Stated AI-use disclosure policy
   
4. **PR description** (read fourth, compare against diff and plan):
   - Title and full description text
   - Note any claims about scope, completeness, or behavior
   
5. **Test evidence** (read fifth, compare against plan's test plan):
   - Before/after output, screenshots, or console transcripts
   - Repo's own checks' outcomes (pytest, CI run, etc.)
   
6. **Commits and thread** (read sixth):
   - Commit messages for clarity and relevance
   - Any maintainer guidance or notes in the thread

## Evidence gathering

For each check, gather evidence as follows:

**Plan fidelity check:**
- Gather: Plan's Files list, Plan's Approach steps, Plan's Scope/Not-in-scope statements, Diff's changed files list, Description's claims about what the PR does, Plan's deviation notes
- Record: Which files in diff are outside plan's Files list; which plan steps are missing from diff; which description claims differ from diff

**Test evidence check:**
- Gather: Plan's test-plan section (the scenario names and expected behavior), Test-evidence section's before/after outputs, Repo-checks' outcomes (passed/failed)
- Record: Which test scenarios from plan are shown in test-evidence; whether before and after outputs are observable and concrete; whether repo checks passed

**Diff quality check:**
- Gather: Diff's code hunks, commit messages
- Record: Presence of debug prints, commented-out lines, dead code, reformat-only hunks; commit message clarity

**Standards and disclosure check:**
- Gather: Repo-facts template section, Repo-facts AI policy, PR description text
- Record: Which required template sections are filled vs. skipped; whether AI-disclosure is present if policy requires it

**Description fidelity check:**
- Gather: PR description claims, Diff contents
- Record: Which description claims are contradicted by diff; which scope statements in description don't match plan or diff

**Honest deferral check:**
- Gather: Plan's deviation notes, Description text
- Record: Whether deferred parts are restated in description using plan's language

## Check execution

Execute checks in rubric order (Plan fidelity → Test evidence → Diff quality → Standards and disclosure → Description fidelity → Honest deferral).

For each check:
- Apply the pass condition using the gathered evidence
- If evidence for the check is genuinely absent (e.g., no test-evidence section in the package), the check fails (unclear becomes fail per the verdict rule)
- If evidence is present but ambiguous, apply the decision rule (does it pass or fail the condition stated in the rubric)
- A "pass" on a required check or a "preferred" check advances to the next check
- A "fail" on a required check stops the rubric (that check is the deciding check); continue grading remaining checks only to provide full output

## Verdict assembly

1. For each required check in rubric order, record pass or fail
2. If any required check fails, the verdict is **reject**; if all required checks pass, the verdict is **accept** (preferred checks never change this)
3. The deciding check is the first required check that failed (if verdict is reject) or the last check executed (if verdict is accept)
4. In the JSON output, quote the evidence or fact from the deciding check that determined the verdict: the brief statement from the rubric's pass condition or the specific evidence from the package
