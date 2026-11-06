## Environment

Fedora Linux 44 (kernel 7.2), Node.js v24.15.0, npm 11.12.1, playwright 1.56.1 (repo-pinned) with its Chromium Headless Shell 141.0.7390.37 (`npx playwright install chromium`), and ffmpeg 8.1.3. My first run used Fedora's `ffmpeg-free`, which has no `libx264`; I later swapped it for RPM Fusion's `ffmpeg` (`sudo dnf swap ffmpeg-free ffmpeg --allowerasing`) and re-ran everything, and the results below are from that build. html-anime at commit `6bdd40b` (`main` after PR #18). Install: `npm install --ignore-scripts`; `npm test` passes on this commit.

## Steps to reproduce

`renderShots()` only accepts shot ids like `scene-001` (`validateTimeline` requires `/^scene-\d{3,}$/`) and loads `<htmlDir>/<shot.id>/index.html`, so `films/roof/index.html` has to be placed in that layout:

1. Generate a valid `timeline.json` in a temp dir: `node harness/preview-film.mjs examples/moonlight-promise.json /tmp/h9-repro/preview`
2. Copy `/tmp/h9-repro/preview/timeline.json` to `/tmp/h9-repro/roof-timeline.json` and edit it to one shot: `id: "scene-001"`, `frames: 192`, `seconds: 8`, `startFrame: 0`, `endFrame: 192`, `totalFrames: 192`, `totalSeconds: 8` (the roof film is 8 s, 1920x1080, 24 fps).
3. Copy the reference film in: `mkdir -p /tmp/h9-repro/roof-html/scene-001 && cp films/roof/index.html /tmp/h9-repro/roof-html/scene-001/index.html`
4. Render it: `node harness/render-film.mjs /tmp/h9-repro/roof-timeline.json /tmp/h9-repro/roof-html /tmp/h9-repro/out`
5. Observed: `scene-001: HTML composition metadata or GSAP timeline invalid` (exit code 1), after about 3.5 s, before any frame is rendered.
6. Control: in a copy of the same HTML, change only `data-composition-id="roof"` to `"film"` (line 56) and `window.__timelines["roof"]` to `["film"]` (line 169), and run step 4 on it. It prints `Rendered scene-001: 192 frames`.

## Evidence

- Output of step 5: `scene-001: HTML composition metadata or GSAP timeline invalid`.
- Output of step 6 on `main`: `Rendered scene-001: 192 frames`. (On my first run, before the ffmpeg swap, the control got past the metadata check and then stopped at `Unknown encoder 'libx264'`.)
- `films/roof/index.html:56` declares `data-composition-id="roof" data-duration="8" data-width="1920" data-height="1080"` and `:169` registers `window.__timelines["roof"]`, so duration and size already match the timeline; the id is the only difference from the control.
- `harness/render-film.mjs:50` and `:55` look for `[data-composition-id="film"]` and `window.__timelines?.film`; `:76` seeks `window.__timelines.film`.
- My findings: the failure is the hardcoded id. With the id changed to `film` the same shot renders on `main`, so nothing else about the roof film is the problem. Commit showing reproduction: none.
