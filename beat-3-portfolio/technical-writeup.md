# I fixed a hardcoded id that stopped html-anime from rendering its own sample film

## Overview

html-anime is a small open source tool that turns HTML scenes into video using a headless browser and ffmpeg. Its renderer looked for a scene whose id was exactly `film`, so the project's own sample film, `films/roof`, failed before it drew a single frame. I found the issue (#9), reproduced it, changed the renderer to read the id from each scene's HTML, and opened PR #21: https://github.com/blixvip/html-anime/pull/21. The maintainer, blixvip, merged it on 2026-11-06, the day I opened it. I wrote the renderer change myself. Claude Code helped me trace the cause, wrote the test, ran the checks and drafted my write-ups, which I reviewed and edited. The scout tool I used to find the issue is also mine: I wrote its search strategy and rubric.

## What happened

**Finding an issue that was actually free.** My issue scout is a Claude Code skill that searches GitHub for first-contribution issues and grades them against a rubric. On 2026-11-04 I ran it by hand, and all of its top five picks were rejected. Three were rejected because an open pull request already fixed the issue. The cause was in my own search strategy: the ranking read only the search result fields, which say nothing about linked PRs, so claimed issues ranked as high as free ones. I added a filter to every query:

```
Before: ... --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url
After:  ... --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,createdAt,url -- "-linked:pr"
```

I also added a rule that drops issues created before 2025-11-04, because `updatedAt` is bumped by bot comments and PR cross-references. On 2026-11-05 I chose html-anime#9 and commented to claim it.

**Reproducing the bug.** The renderer only accepts shot ids like `scene-001`, so I put the roof film into that layout and ran the real render command:

```
$ node harness/render-film.mjs roof-timeline.json roof-html out
scene-001: HTML composition metadata or GSAP timeline invalid
```

It exited with code 1 after about 3.5 seconds. As a control I changed only the composition id in a copy of the same HTML to `film`, and it rendered `Rendered scene-001: 192 frames`. That showed the id was the only problem. My first control run also failed, with `Unknown encoder 'libx264'`, because Fedora's `ffmpeg-free` has no H.264 encoder. I swapped in RPM Fusion's ffmpeg and redid all my evidence.

**The fix and the test.** In `harness/render-film.mjs` I replaced the hardcoded lookup with the id each scene declares:

```diff
-          const root = document.querySelector('[data-composition-id="film"]');
+          const root = document.querySelector("[data-composition-id]");
+          const id = root?.dataset.compositionId;
...
-            hasTimeline: Boolean(window.__timelines?.film)
+            hasTimeline: Boolean(id && window.__timelines?.[id])
```

The timeline seek had the same hardcoded name, so it now uses `window.__timelines[id]` too. Claude Code wrote the test, `harness/film-composition.test.mjs`, which checks that the roof film declares its id and registers its timeline under that same id:

```js
assert.equal(id, "roof");
assert.ok(html.includes(`window.__timelines["${id}"]`), "roof film registers its timeline under its own id");
```

To check the test could fail, I changed the roof film's timeline key to `"other"` and it failed with `AssertionError: roof film registers its timeline under its own id`. After the fix the repro printed `Rendered scene-001: 192 frames`, and `npm test` and the full `npm run film:full` render (1,440 frames, H.264, 1920x1080) both passed.

**How it ended.** The maintainer, blixvip, merged PR #21 into html-anime's `main` branch on 2026-11-06, the day I opened it. The PR description carried `Fixes #9` and spelled out the limit of my test, so the maintainer could see what the change did and did not prove. As a result, the renderer on `main` now reads the id each scene declares, and the project's own `films/roof` sample renders instead of stopping at `HTML composition metadata or GSAP timeline invalid`.

## Limitations and what's next

The test checks the roof film's HTML, not the renderer, so it would also pass on the old code. It proves the sample film follows the rule, but it doesn't prove the renderer handles other ids. A real regression test would render a scene with a non-`film` id and check the result, and that is what I would add next.

My scout still has a gap I found but did not fix. On 2026-11-05 it recommended pr-agent#4685, which looked free, but a PR for it appeared about 90 minutes after the issue was opened. I caught that miss myself. I proposed a rule to treat "not claimed" as unproven on issues under 48 hours old, but I haven't added it yet, so the scout could still make the same mistake. Next I would add that rule and have the scout recheck for linked PRs right before I claim an issue.
