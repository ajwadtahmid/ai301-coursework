# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

**Where it lives:**
- Eval: Plan context's Files and Scope statements, Candidate PR's diff (list of changed files), PR description (claimed scope and behavior changes)
- Live: plan.md's Files and Scope sections + deviation notes, branch diff (`git diff main...HEAD`), draft PR description

**What good looks like:**
- Every file in the diff's changed-files list appears in the plan's stated Files section, OR the plan's deviation notes explicitly call out that file by name as an exception
- The approach steps named in the plan are visibly present in the diff hunks (concrete changes, not unrelated edits)
- The description's stated scope and behavior match the diff's actual behavior (if description claims "implements the plan exactly", the diff contains exactly the plan's files and no others; if description claims a partial fix, the plan's deviation notes make that explicit)
- Silent drift in either direction (doing more or less than planned with no note) shows as changed files unlisted in plan, or description claims not matching diff

## Test evidence (harness category: not-tested)

**Where it lives:**
- Eval: Test evidence section's before/after outputs, repo-facts block's CI/test outcomes, plan context's test-plan statement
- Live: Your captured before/after test runs, repo's own test suite outcome, plan.md's test-plan section

**What good looks like:**
- The plan's test scenario (the specific issue-repro steps or failure mode named) is re-run with observable before/after: console output, screenshot, or transcript showing the symptom before and the fixed behavior after
- The expected-after state is named concretely (not "tests pass" alone, but "the command exits 0 and outputs X" or "the UI renders Y")
- Repo's own checks (pytest, cargo test, CI pipeline) show a passing outcome (green, all tests pass, no errors)
- When the plan names multiple failure modes or test scenarios, each appears in the evidence section; when plan names one scenario, the evidence exercises that one
- Missing before/after or "tests pass" with no observable behavior fails

## Diff quality (harness category: unreviewable)

**Where it lives:**
- Eval: Candidate PR's diff hunks and commit messages
- Live: Branch diff and commit messages

**What good looks like:**
- The fix is cleanly visible: the changed logic or correction appears in the diff hunks with minimal surrounding code
- No debug prints, console.log, eprintln!, println! left in the code
- No commented-out lines, commented-out attempts, or `// TODO` comments
- No dead functions, `#[allow(dead_code)]` blocks, or unreachable code
- No formatting-only hunks (pure whitespace, re-indent not tied to the fix, blank-line insertions)
- No unrelated file edits (imports restructured for style, unrelated flags rewording, new features bundled in)
- Commit messages are clear: "fix(directory): render logical path when contract_repo_path returns None" not "wip" or "fmt + cleanup"
- A multi-commit PR groups related changes: each commit name reflects its content

## Standards and comms (harness category: standards-wall)

**Where it lives:**
- Eval: Repo-facts block's PR template and stated policy, candidate PR's description text, repo-facts block's AI disclosure policy
- Live: Repo's PR template (on GitHub, CONTRIBUTING.md, or repository settings), PR description, stated AI policy

**What good looks like:**
- Every template section marked "required" is filled with real content: a PR asking for "closes #XXXX" has that line with an actual issue number; a PR asking for "type of change" has one named (Bug fix, Feature, Docs); a changelog entry is present if required
- No boilerplate or skipped sections: if template says "Description: describe your fix", the description field is filled; if it says "Checklist", items are checked or explained, not left blank
- If the repo's stated policy requires AI-use disclosure, the description contains a clear statement naming that AI assistance was used; absence of required disclosure fails
- No visible ignorance of maintainer requests (e.g., "please add this test case" mentioned in the issue thread but test is absent)
- Compliant PRs fill sections with specifics and show evidence of template engagement
