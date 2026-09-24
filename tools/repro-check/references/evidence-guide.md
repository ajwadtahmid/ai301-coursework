# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: week 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; your operator swap showed you what that feels
like. Write the map you wish your executor had.
-->

## Environment

The environment section of the repro report lists the versions, OS, and runtime details that define the conditions under which the reproduction was attempted. In eval bundles, this lives in the "Candidate repro report" section. A sufficient environment record names the exact version of the tool tested (matching or differing from the issue's stated target), the OS or platform, and any major runtime dependency versions that the issue's context suggests matter (e.g., Python version, library versions mentioned in the thread, or environment variables for locale-sensitive issues). Version deltas are acceptable if named: "tested 1.5.3 when the issue reports 2.0" counts as recorded; "no version delta mentioned" when the issue's own report targets a specific release does not.

## Steps

The steps section describes how to trigger the bug, starting from a clean state. In eval bundles, steps appear in the repro report. Steps are followable by a stranger if they: (1) name the starting state (directory contents, initial config, or connection state), (2) give exact commands or actions that trigger the reported behavior without skipping the trigger condition, and (3) identify which step's output shows the failure. Steps that say "run X, you'll see Y" but omit a critical flag or prior setup that the issue describes are incomplete. Steps that generalize to adjacent behaviors the issue does not describe, or steps that run a different command than the issue names, are wrong-target.

## Behavior shown

Artifacts are the evidence of what happened: command output, log excerpts, screenshots, or error messages. In eval bundles, artifacts appear in the repro report section after the steps. An artifact shows the issue's behavior if it matches the symptoms the issue names, without generalization. If the issue says "crash with exit code 101" and the artifact shows "exit code 1", that is adjacent, not matching. If the issue says "data corrupted" and the artifact shows "data written successfully", that is no-evidence. If the issue is unspecified ("something is weird") and the artifact shows the tool running, that is not evidence of the weirdness. An honest cannot-reproduce is a valid artifact: "ran the exact steps, no failure observed; here is my environment and hypothesis" is evidence, not absence.

## Honesty

The repro report's summary or closing statement should match what the artifacts show. If the artifacts show a graceful error (exit 1, a helpful error message) and the report says "this crash destroys data," that is claiming more than evidence supports. If the report says "I verified this race condition" with no artifact showing the race (timing output, concurrent-execution logs, or similar), that is assertion without backing. An honest report states what happened: "I ran the exact steps and got a parse error" (backed by a parse-error artifact) is honest; "I ran the exact steps and saw the crash mentioned in the issue" (with that same parse-error artifact) is dishonest because parse error ≠ crash. The claim comment should align with the repro report: "I will investigate the root cause" is honest; "I have fixed the root cause" before any repro is done is not.

## Comms

The claim comment is the first words on the thread. It should name the specific issue it addresses, state what the author will do next (not what they have already fixed), and respect the repo's stated template if one exists. Some repos ask for confirmation the issue is reproducible; some ask for a step-by-step account. The repo's stated contribution policy may require disclosing AI assistance. In eval bundles, the repo's policy statement appears under "contribution policy" in the Repo facts section; the claim comment is under "Candidate claim comment." A specific claim names the issue (not just "+1") and names next steps that match the repro report (not guarantees like "fixed in 2 days"). Boilerplate like "I will resolve this" without reference to the issue's specifics fails specificity. An AI-use disclosure is required if the repo's policy says "disclose all AI usage" or "disclose if AI-assisted"; if the policy is silent or permissive, disclosure is not required.
