# Contribution: Read the composition id from the scene HTML instead of hardcoding "film"

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Ajwad Tahmid
**GitHub Username:** ajwadtahmid
**Issue:** https://github.com/blixvip/html-anime/issues/9
**Pull Request:** https://github.com/blixvip/html-anime/pull/21

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Scout installed, search strategy written, four scans, three of them by hand**

I installed the scout over my unit-1 skill and wrote `search-strategy.md` with four `gh` queries and six ranking rules, each tied to a sentence of my fit profile. My one `claude -p` scan hit its $1.50 budget cap and printed nothing, so I ran the same funnel by hand with `gh` and a short script instead. The first scan's top five were all rejected, and three had open PRs that already fixed them because search results don't show linked PRs, so I added `-linked:pr` to every query and a rule dropping issues created over a year ago. My rubric also passed issues that had a stranger's claim comment (it only reads assignees and open PRs), which I logged but didn't patch that week.

**Unit 6: Voice guard and repo memory installed after I had already posted my claim**

I posted my claim through the browser before the voice guard existed, so the guard has one wiring-test stop and one after-the-fact manual pass on the claim text in its log, and has never caught a rule of mine. I added an Upstream section to my voice guide (claims, questions, pings) and wrote a 27-line `CLAUDE.local.md` for html-anime from `CONTRIBUTING.md` and a look at `render-film.mjs`; `npm install` created a `package-lock.json` that I had to exclude locally so it wouldn't show up in the diff. I also replaced my rubric's strict scope check with "Scope is feasible" plus a difficulty note, after it rejected 5 of the 10 issues in my first two scans.

**Unit 7: Ship-gate installed, manifest written, gate passed on the first run**

I installed the ship-gate with its one-command install and wrote a manifest from `CONTRIBUTING.md` and the PR template (`npm install --ignore-scripts`, `npm test`, and `npm run film:full`), first excluding `film:full` with a dated reason because my ffmpeg had no `libx264`, then making it a required check after I swapped in RPM Fusion's ffmpeg. The hook test showed both stops (the voice guard, then `ship-gate: STOP.`), and on my real branch the gate ran the repo checks and then my pr-precheck and printed `ship-gate: PASS`, with all five checks and the honest-deferral check passing and no voice-guide notes on that first run (a later run noted my environment sentence sat mid-sentence, which the guard still passed). It passed first time because plan-check had already rejected my first plan for a missing "Unknowns and risks" section; its limits for me are that the pass lasts 30 minutes and any edit resets it, so I re-run it before opening the PR, and that the manifest's slow `film:full` row (about 2 minutes) makes each run longer.

**Unit 8: PR monitor installed, playbook written, first run said wait**

I installed the PR monitor (a folder copy, no hook), wrote a four-section playbook for html-anime, and ran `--state` and then a full run on PR #21 within hours of opening it. `--state` read the PR as mergeable with no checks, no reviews and no comments, and the full run drafted "Wait: nudge threshold not met" because my own playbook asks for 5 quiet days, with the earliest nudge date of 2026-11-11, so I sent nothing. What fell short is my threshold's evidence: html-anime has only three merged PRs (one outside fix merged in about 8 hours, a maintainer PR in about 15 hours, and a first feature in 12 days), so I picked 5 days from thin data, and the monitor cannot tell me whether the maintainers are simply away. I did not change anything yet; if a review arrives I will rewrite its draft into my own words and post it through my voice guard, and I would add a second-signal rule (recent commits) to the monitor's state check.

---

## Why I Chose This Issue

I looked through more than a few issues on the Issue Selection tracker; many were old, and most of the new ones were already claimed. I picked three to grade in direct mode (HyP3 #2551, DNA #255 and DrumBeatRepo #627): HyP3 and DrumBeatRepo were accepted and DNA was rejected because an open PR already implemented it. HyP3 had a stranger's claim comment that my rubric missed, and DrumBeatRepo #627 was claimed by someone else before I could. I then ran my scout's funnel on all of GitHub, by hand because my `claude -p` scan hit its budget cap, and widened the queries until I got candidates; I graded pr-agent #4685, but a PR for it appeared about 90 minutes after the issue, so I dropped it. I chose html-anime #9 (https://github.com/blixvip/html-anime/issues/9). It came from my own scan, not from the site, and the scout's verdict was accept (all six checks passed; under my patched rubric the difficulty note is straightforward).

I chose it because the repo had several open issues with help wanted, it was recent, and I watch anime, so a project that draws anime scenes from HTML was relevant to me. It also fits because it is JavaScript, one bounded fix in `harness/render-film.mjs` (replace a hardcoded `film` id with the id read from the scene HTML) plus a test, and the issue names the file and lines. I hope to learn how a small project reviews outside PRs and how to write a test that fits their plain `node:assert` style. The repo looks young and AI-assisted (it ships `AGENTS.md` and `CLAUDE.md`), and its maintainer merged the last outside PR within about 8 hours; there is a risk someone else claims or fixes it first, which is why I claimed quickly.

My claim comment: https://github.com/blixvip/html-anime/issues/9#issuecomment-6647600820 (posted 2026-11-05).

Where the scout fell short on the wild: the "Not claimed" check ignores claim comments and linked-PR information is not in the search results, so it accepted issues a stranger had already taken (HyP3) or that had a fix in flight (doc-kit, busboy, apipkg); the "Maintainer is present" check can be satisfied by bot commits; and for brand-new issues "not claimed" is weak evidence (pr-agent). I fixed the queries (`-linked:pr`, a one-year age cutoff) and the scope check, and logged proposals for the claim-comment and bot-commit checks that I have not patched.

**Toolkit**

- Tool: issue-scout, with my own search strategy (four `gh` queries, six ranking rules) and my unit-1 rubric, patched once this week; I ran its funnel by hand after the `claude -p` scan hit its $1.50 cap.
- What it produced: three by-hand scans (the first two graded 5 issues each and rejected all ten; the third graded 4 and accepted them) plus direct grades of my three tracker picks; html-anime #9 got accept with a difficulty note of straightforward.
- Difficulties: search results can't show linked PRs, stars or claim comments, so the ranking passed issues that were already taken; `-linked:pr` fixed the PR half; the claim-comment gap and bot commits are still in my rubric.
- What changed: `-linked:pr` and a one-year age rule in the strategy; "Scope is bounded" replaced by "Scope is feasible" plus a difficulty note; claim-comment and bot-commit patches proposed in `scan-notes.md`, not applied.

---

## Understanding the Issue

**Problem Description**

The renderer in html-anime (`renderShots()` in `harness/render-film.mjs`) only works on scene HTML whose composition id is exactly `film`. It looks for `[data-composition-id="film"]` and `window.__timelines.film`, so a hand-written film with a different id, like the repo's own reference film `films/roof/index.html` (id `roof`), is rejected with "HTML composition metadata or GSAP timeline invalid".

**Expected Behavior**

The renderer should read the id the scene HTML declares on its root `data-composition-id` element and look up the timeline under that same key, so any hand-authored film renders, and a test should show that `films/roof/index.html` meets the renderer's metadata rules.

**Current Behavior**

Rendering `films/roof/index.html` through `node harness/render-film.mjs` fails after about 3.5 seconds with `scene-001: HTML composition metadata or GSAP timeline invalid` (exit code 1), before any frame is rendered. If I change only the id to `film` in a copy of the file, the same check passes.

**Affected Components**

`harness/render-film.mjs` (the metadata check at lines 49-60 and the frame-seek loop at lines 75-79), `films/roof/index.html` (the reference film that declares id `roof`), the `npm test` chain in `package.json`, and `harness/preview-film.mjs` (generates scene HTML with id `film`, which must keep working).

**Toolkit**

- Tool: No scout or checker applies here; I used Claude Code to read `harness/render-film.mjs` and `films/roof/index.html` and check my four answers against the repro.
- What it produced: The four answers above, tied to line numbers (render-film.mjs:49-60, 75-79; roof/index.html:56, 169).
- Difficulties: none met.
- What changed: nothing.

---

## Reproduction Process

**Environment Setup**

Fedora Linux 44 (kernel 7.2), Node.js v24.15.0, npm 11.12.1, the repo-pinned playwright 1.56.1 with its Chromium Headless Shell 141.0.7390.37 (`npx playwright install chromium`, about 104 MB), and ffmpeg 8.1.3. I cloned my fork (`ajwadtahmid/html-anime`) at upstream commit `6bdd40b` (`main` after PR #18), ran `npm install --ignore-scripts`, and `npm test` passes on that commit. Challenges: the renderer only accepts shot ids like `scene-001` (`validateTimeline` requires `/^scene-\d{3,}$/`), so my first attempt with a shot named `roof` stopped with "Noncontiguous timeline" before it ever tested the bug, and I had to put the roof film in the `scene-001` layout; and my Fedora `ffmpeg-free` had no `libx264` encoder, so my first reproduction could not render a finished MP4. I later replaced it with RPM Fusion's ffmpeg (`sudo dnf swap ffmpeg-free ffmpeg --allowerasing`) and re-ran everything with the real encoder, so the results below that mention rendering come from that build. I preferred Playwright's Chromium to the Brave browser I have installed because the repo pins Playwright and a stranger can rerun it.

**Steps to Reproduce**

1. In the repo root, generate a valid timeline: `node harness/preview-film.mjs examples/moonlight-promise.json /tmp/h9-repro/preview`
2. Copy `/tmp/h9-repro/preview/timeline.json` to `/tmp/h9-repro/roof-timeline.json` and edit it to one shot: `id: "scene-001"`, `frames: 192`, `seconds: 8`, `startFrame: 0`, `endFrame: 192`, `totalFrames: 192`, `totalSeconds: 8` (the roof film is 8 s, 1920x1080, 24 fps).
3. Put the reference film in the layout the renderer reads: `mkdir -p /tmp/h9-repro/roof-html/scene-001 && cp films/roof/index.html /tmp/h9-repro/roof-html/scene-001/index.html`
4. Render it: `node harness/render-film.mjs /tmp/h9-repro/roof-timeline.json /tmp/h9-repro/roof-html /tmp/h9-repro/out`
5. Observed: `scene-001: HTML composition metadata or GSAP timeline invalid`, exit code 1, after about 3.5 seconds and before any frame is rendered.
6. Control: in a copy of the same HTML change only `data-composition-id="roof"` to `"film"` (line 56) and `window.__timelines["roof"]` to `["film"]` (line 169), and run step 4 on it. The metadata check passes and the renderer renders the shot (`Rendered scene-001: 192 frames`).

**Reproduction Evidence**

- **Commit showing reproduction:** none (the repro uses only files in `/tmp`; no code was changed)
- **Screenshots/logs:** step 5 output is `scene-001: HTML composition metadata or GSAP timeline invalid`. On my first run the control ended with ffmpeg's `Unknown encoder 'libx264'` (my machine's missing encoder); after installing a libx264 ffmpeg the control prints `Rendered scene-001: 192 frames` on `main`.
- **My findings:** the failure is the hardcoded id. `films/roof/index.html:56` declares `data-composition-id="roof" data-duration="8" data-width="1920" data-height="1080"` and `:169` registers `window.__timelines["roof"]`, so the duration and size already match the timeline; `harness/render-film.mjs:50` and `:55` look for `[data-composition-id="film"]` and `window.__timelines?.film`, and `:76` seeks `window.__timelines.film`. With the id changed to `film` the shot renders on `main`, so the id is the only difference between the two runs.

**Toolkit**

- Tool: repro-check, in live mode on my claim and my repro report, with my voice guide and the unit-6 wild scope.
- What it produced: accept on all five checks (environment, steps, behavior matches the issue, honesty, AI disclosure); no voice-guide rule was broken.
- Difficulties: its one note was that step 2 asks a stranger to hand-edit JSON rather than giving a command; I also found my first timeline failed on the `scene-NNN` id rule, which is a detail the issue does not mention.
- What changed: nothing in the tool; I wrote the `scene-001` layout requirement into the steps so a stranger does not hit the same wall, and I left the hand edit in step 2 as it is.

---

## Solution Approach

**Analysis**

In `harness/render-film.mjs`, `renderShots()` opens each shot's HTML and runs one `page.evaluate` that calls `document.querySelector('[data-composition-id="film"]')` and reads `window.__timelines?.film` (lines 49-60). The frame loop then calls `window.__timelines.film` again to pause and seek (lines 75-79). So the id `film` is hardcoded in three places and nothing reads the id the scene HTML declares. `harness/preview-film.mjs` generates scenes with id `film`, which is why the generated pipeline works and the hand-authored `films/roof/index.html` (id `roof`, declared at line 56 and registered at line 169) does not. I confirmed this by rendering `roof` (fails) and a copy with the id changed to `film` (passes the metadata check).

**Proposed Solution**

Read the id from the page: select the root with `[data-composition-id]`, read its `dataset.compositionId`, check `window.__timelines[id]` exists, return the id with the other metadata, and pass that id into the seek `page.evaluate` instead of `film`. Add a test that reads `films/roof/index.html` and asserts it declares a composition id, the duration, width and height the renderer checks, and registers `window.__timelines` under that same id. In scope: reading the composition id from the scene HTML in `renderShots()` and one test for the roof film's metadata.

Not in scope: `harness/preview-film.mjs` and the `film` id it generates, `validateRenderConfig`, the `scene-NNN` shot id rule, the GSAP URL handling (issue #8), file URL building (issue #6/#19), the move to `node --test` (issue #7), and any docs.

**Implementation Plan**

1. Modify `harness/render-film.mjs`: replace the hardcoded `film` selector and `__timelines.film` lookups with the id read from the root element, and use that id in the seek loop. Keep the existing error message.
2. Add `harness/film-composition.test.mjs`: a plain `node:assert` test, in the style of the other `harness/*.test.mjs` files, that reads `films/roof/index.html` and asserts the metadata contract (declared id, `data-duration`, `data-width`, `data-height`, and `window.__timelines[<same id>]`).
3. Modify `package.json`: add the new test file to the `npm test` chain.
4. Verify: re-run the reproduction steps before and after, confirm the roof film now passes the metadata check and reaches the ffmpeg step (my ffmpeg has no `libx264`, so no finished MP4 here), and run `npm test`.

Files touched: `harness/render-film.mjs`, `harness/film-composition.test.mjs` (new), `package.json`.

Test scenario: the repro (render `films/roof/index.html` through `renderShots`) printed `HTML composition metadata or GSAP timeline invalid` before the change; after the change the output no longer contains that message: on this machine, whose ffmpeg has no `libx264`, the render should instead stop at ffmpeg's `Unknown encoder 'libx264'` (exit code 1), which is the same place the `film`-id control already stops. A render of a `film`-id scene must still get past the metadata check the same way. The new test checks the roof film file itself and passes before and after the change; it does not exercise the renderer, since that needs Chromium, which `npm test` does not install.

Note on the test: the repo's `npm test` is a hand-maintained `&&` chain (issue #7 proposes `node --test`), so a new test file only runs if added to that chain.

**Unknowns and risks**

- Unknown at first: I could not render a finished MP4 (my ffmpeg had no `libx264`), so I did not know whether the rest of `renderShots()` works for `roof`. Resolved while building, see the deviations section.
- `document.querySelector("[data-composition-id]")` takes the first matching element. Unknown whether any film nests several composition ids; the issue and the reference film have one.
- The new test checks the roof film file with plain string matching, so it passes before and after the fix and would not catch a renderer regression. A test that drives the renderer needs Chromium, which `npm test` does not install; unknown whether the maintainer wants one.
- Adding the test to the `npm test` chain may conflict with issue #7, which proposes switching to `node --test`; if #7 lands first I will rebase.
- Not yet known whether the maintainer accepts a static test for the "renderer's metadata contract" or expects something else.

**Deviations from the plan**

None to the files or approach. Two things changed while building:

- The libx264 limit is gone. First I used a temporary ffmpeg wrapper that swapped `libx264` for `mpeg4` (outside the repo), which showed `roof` renders end to end after the fix; then I replaced my system ffmpeg with the RPM Fusion build that has libx264 and re-ran everything with the real encoder: `roof` renders 192 frames after the fix (and fails on main), and `npm run film:full` renders 1440 frames. So the test scenario's "after" output is now `Rendered scene-001: 192 frames`, not the ffmpeg encoder error, and `film:full` is a required check in the ship manifest instead of excluded.
- PR #19 edits the same `package.json` test chain and `render-film.mjs` (different lines), so a rebase may be needed.

**Toolkit**

- Tool: plan-check, on `plan.md` and `plan-comment.md` (my plan-check procedure and rubric, with the unit-6 wild scope: the comment stays off the thread).
- What it produced: A first run of reject and a second of accept, in the plan-check output format with six checks.
- Difficulties: My plan had no "Unknowns and risks" section, which my procedure grades as unclear and so a fail; I would not have caught it by rereading, and the scope check also wanted explicit In scope and Not in scope lines.
- What changed: I added "In scope", "Not in scope", an Unknowns and risks section, and the exact output I expected after the fix, then re-ran and got accept. During the build one unknown resolved (roof does render end to end), recorded under deviations in `plan.md`.

---

## Testing Strategy

Before and after, I re-run the reproduction: render `films/roof/index.html` as one `scene-001` shot with `node harness/render-film.mjs`, on Node 24, playwright 1.56.1 and ffmpeg 8.1.3 with libx264. On `main` (`6bdd40b`) it prints `scene-001: HTML composition metadata or GSAP timeline invalid` and exits 1 after about 3.5 s. After the fix it prints `Rendered scene-001: 192 frames` and `ffprobe` shows h264, 1920x1080, 24 fps, 192 frames; a copy of the roof HTML with its id changed to `film` renders both on `main` and after the fix. The repo's own checks pass: `npm test` runs five test files, including the new `harness/film-composition.test.mjs` (when I temporarily changed the roof film's registered id to `other` that test failed, and I restored the file), and `npm run film:full`, the check the PR template asks for when the harness changes, renders 3 shots, 1440 frames, 60 s at 1920x1080. The new test reads the film file and does not drive the renderer, so it also passes on `main`; the renderer's change is shown by the before and after renders, not by it. My first before-and-after run was on a machine whose ffmpeg had no libx264, where the render stopped at the encoder; the numbers above are from the re-run after I installed a full ffmpeg. The full outputs are in `test_evidence.md` in my clone (not committed).

**Toolkit**

- Tool: `npm test` and `npm run film:full` from the repo (the checks named in `CONTRIBUTING.md` and the PR template) and my own before/after renders; the ship-gate ran `npm install --ignore-scripts`, `npm test` and `npm run film:full` from my manifest before the PR opened.
- What it produced: Five passing test files, `npm run film:full` rendering 1440 frames, the roof render before (fails) and after (renders 192 frames), and `test_evidence.md`.
- Difficulties: this machine's ffmpeg had no `libx264`, so `npm run film:full` could not finish at first (I fixed that by swapping to RPM Fusion's ffmpeg), and my new test checks the film file, so it does not fail without the fix.
- What changed: I first excluded `film:full` in the manifest with a dated reason and showed an mpeg4 stand-in; after installing a libx264 ffmpeg I made `film:full` a required manifest check and re-ran everything with the real encoder. The limit that remains, that the new test does not fail without the fix, is stated in the PR description.

---

## Implementation Notes

**2026-11-05**

Claimed the issue through the browser, then installed the voice guard and cloned my fork. Claude Code gave me ideas along the way (the approach for the fix and the test), and I wrote the `render-film.mjs` change myself. Reproduced the failure on `roof`: my first attempt failed with "Noncontiguous timeline" because `validateTimeline` only accepts shot ids like `scene-001`, so I put the roof film in that layout. Found that my machine's ffmpeg has no `libx264`, so a full render is impossible here; the metadata check, not the encoder, is what the issue is about.

**2026-11-05**

Wrote the plan, ran plan-check (reject first for a missing "Unknowns and risks" section, then accept), and built the fix on `fix/9-composition-id` in two commits: the renderer change, then the test and the `package.json` chain. Chose the test file name `film-composition.test.mjs` instead of `render-film.test.mjs` so it does not collide with issue #7, which plans a file with that name. Wrong turn avoided: a test that drives Chromium would fail without the fix, but `npm test` does not install a browser, so I kept the test static and said so in the PR. Noticed open PR #19 changes the same test chain in `package.json`; a rebase may be needed.

**2026-11-05**

Removed the encoder limit: swapped my system `ffmpeg-free` for RPM Fusion's `ffmpeg` (with libx264) and redid the evidence. `roof` renders 192 frames after the fix and still fails on `main`, and `npm run film:full` renders all 1440 frames, so `film:full` became a required row in my ship manifest, and I rewrote `test_evidence.md` and the PR description to use the real encoder instead of my earlier mpeg4 stand-in.


**Toolkit**

- Tool: Claude Code for reading the code, suggesting the approach, writing the test and running the checks and renders; I wrote the change in `render-film.mjs` myself; git for the branch and two commits.
- What it produced: Two commits on `fix/9-composition-id` (+23 -6 over three files) and the notes above.
- Difficulties: `npm install` created a `package-lock.json` that I had to keep out of the diff with `.git/info/exclude`; `git stash -u` while comparing before and after on `main` also briefly moved my new test file.
- What changed: nothing.

---

## Pull Request

**PR Link:** https://github.com/blixvip/html-anime/pull/21

**PR Description:** Opened 2026-11-06 from `ajwadtahmid:fix/9-composition-id` into `blixvip/html-anime` `main`, title "Read the composition id from the scene HTML in the renderer", `Fixes #9`, two commits (the renderer change, then a test added to the `npm test` chain). The description says what changed and why: `renderShots()` only accepted scene HTML with the id `film`, so the repo's own `films/roof/index.html` failed with `HTML composition metadata or GSAP timeline invalid`; it now reads the id from the root's `data-composition-id` and uses it for the timeline lookup and the seek, and generated `film` scenes are unaffected. It gives my before and after: rendering `roof` on `main` fails (exit 1, no frame rendered) and after the fix prints `Rendered scene-001: 192 frames` (h264, 1920x1080, 24 fps), a copy with id `film` renders both before and after, `npm test` passes all five test files, and `npm run film:full` renders 3 shots, 1440 frames, 60 s. It says plainly that the new test checks the film file and does not exercise the renderer, so it also passes on `main`; it notes that PR #19 edits the same test chain, so one of the two may need a small rebase; and it discloses that Claude Code helped trace the id, suggested the approach, wrote the test, ran the checks and renders and drafted the description, while I wrote the `render-film.mjs` change and reviewed and edited the test and the description.

**Maintainer Feedback:**

- 2026-11-06: none yet. The PR was just opened.

**Status:** Awaiting review

**Toolkit**

- Tool: the ship-gate (my manifest of `npm install --ignore-scripts`, `npm test` and `npm run film:full`, then my pr-precheck) and the voice guard, both on the `gh pr create` doorway.
- What it produced: the gate ran my three manifest checks (the film render took 122 s) and then pr-precheck on the branch, plan, test evidence and description, and printed `ship-gate: PASS` with all five required checks and the preferred honest-deferral check passing; the voice guard passed the PR text, and the PR opened through Claude Code.
- Difficulties: after I rewrote the fix and re-made the commits the earlier pass no longer applied, so I re-ran the gate by hand (about 2 minutes plus one pr-precheck call) before opening; the gate's report also carried one voice note (the environment sat mid-sentence in my test section) that did not stop the guard.
- What changed: I rewrote the AI paragraph so it says who did what, and I did not change the environment sentence because the guard passed it.

---

## Learnings and Reflections

**Technical Skills Gained**

I learned how to search all of GitHub with `gh search issues` (including the `-linked:pr` qualifier, the 16-owner limit per query, and why result fields alone cannot show stars or claims), how to read an unfamiliar Node project end to end (a manifest becomes a timeline, then scene HTML, then Playwright screenshots, then ffmpeg), and how Playwright selects an element and reads a page's `window` object. I wrote a plain `node:assert` test in the project's style and wired it into a hand-maintained `npm test` chain, and I practiced the fork-and-upstream workflow: a fork clone, a branch, `.git/info/exclude` to keep my own files out of a diff, `gh repo set-default`, and opening a PR through hooks that check it first. I also learned that Fedora's `ffmpeg-free` has no `libx264`, and how to swap it for RPM Fusion's build.

**Challenges Overcome**

Finding an issue nobody had taken was the hardest part: the first two scans rejected all ten graded issues, and three had an open PR already, so I added `-linked:pr` and an age rule and widened the queries. HyP3 and DrumBeatRepo were claimed by someone else, and pr-agent got a PR about 90 minutes after its issue, so I learned to claim fast and to treat an hours-old issue as weak evidence. My first reproduction could not finish rendering because my ffmpeg had no `libx264`, and I fixed that by installing a full ffmpeg and redoing every before and after. The PR gate also made me fix a real gap in my plan (no "Unknowns and risks" section), and writing the PR disclosure made me rewrite the fix myself so the description says exactly who did what.

**What I'd Do Differently Next Time**

I would put one concrete question in my claim comment (whether the test should join the `npm test` chain or wait for `node --test` in issue #7), instead of finding that overlap myself and only noting it in the PR. I would test my scout's cost on one query before running a full scan (my one `claude -p` run hit its $1.50 cap and returned nothing), and I would check the recent merged PRs of the repo before building, so the playbook's wait threshold and my expectations come from data and not from a guess. I would also write the AI-disclosure line at the start of the work, so that what I do myself and what the assistant does is clear while I am doing it and not only at the end.

---

## Resources Used

- html-anime's `CONTRIBUTING.md` and PR template (`.github/pull_request_template.md`): they set the checks (`npm test`, `npm run film:full`), the claim rule, and what the PR description had to answer.
- html-anime issue #9, `harness/render-film.mjs` and `films/roof/index.html`: the issue named the file and lines, and the roof film was the failing case.
- PRs #18, #19 and #20 on html-anime: #18 showed how fast an outside fix is merged, and #19 showed the `package.json` test chain I might have to rebase on.
- The Issue Selection tracker, and the Unit 5 to Unit 8 starter repos (`INSTALL.md` for the voice guard, the ship-gate and the PR monitor): they gave the install steps and the wild scopes for my tools.
- The `gh search issues` flags (`-linked:pr`, `--owner`, `--no-assignee`, `--updated`), which I used to rebuild my search after the first scans failed.
- Playwright's Chromium install (`npx playwright install chromium`) and RPM Fusion's ffmpeg (`sudo dnf swap ffmpeg-free ffmpeg --allowerasing`), which let me render the roof film and run `npm run film:full`.
