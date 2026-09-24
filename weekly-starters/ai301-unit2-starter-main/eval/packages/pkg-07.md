# Eval package: pkg-07

- source: processing/p5.js#7168
- captured: 2026-08-17
- calibration: false

## Repo facts (captured 2026-08-17)

- repo: processing/p5.js (23878 stars, archived: no)
- description: p5.js is a client-side JS platform that empowers artists, designers, students, and anyone to learn to code and express themselves creatively on the web.
- latest release: v2.3.2 (2026-07-30)
- bug reports: template asks for the p5.js version, operating system, web browser and version, actual behavior, expected behavior, and steps to reproduce with a snippet
- contribution policy (CONTRIBUTING.md, section "AI Usage Policy"): fully AI-generated contributions are not accepted; assistive AI use is allowed, and the contributor must understand and take responsibility for every change (details in AI_USAGE_POLICY.md)

## Issue

### FES message is broken if reserved words are used. (#7168)

opened by shibomb (CONTRIBUTOR) on 2024-08-03, state open, labels: Bug

p5.js version: 1.9.4, 1.10.0. Browser: any, with a non-English language (e.g. Spanish, Japanese) first in the browser's language priority settings.

Actual behavior: set the browser's language preference to Japanese; when executing code using reserved words, an error appears in the console, as if the FES message could not be generated:

```
TypeError: Cannot read properties of undefined (reading 'replaceAll')
p5.js translator called before translations were loaded
```

The problem does not occur if the browser's language preference is set to English:

```
🌸 p5.js says: you have used a p5.js reserved function "value" make sure you change the function name to something else.
```

Expected behavior: the translated FES message (or at minimum the English message) should be displayed.

Steps: 1. set the browser's language preference to non-English; 2. add `let value = 0` in the setup function; 3. play.

## Thread highlights (9 comments total)

- 2024-08-12 raclim (CONTRIBUTOR): thanks for reporting; wondering if this could be addressed through the FES
- 2024-08-12 Qianqianye (CONTRIBUTOR): tagged the FES working group to take a look
- 2024-08-13 limzykenneth (MEMBER): looking into this; notes the correct behavior may be to not display the message at all, since `value` is not a top-level defined variable

## Candidate claim comment

Hi! I'd like to investigate this FES translation bug as a first contribution. I reproduced it on 1.11.7 with Japanese language settings (full report below). Per the AI usage policy: I used an AI assistant to help me organize this report; I ran and verified every step myself and I understand what I'm reporting. Next step for me is reading how the translator loads locale files relative to when FES fires.

## Candidate repro report

Environment: p5.js 1.11.7 (CDN single file), Chrome 139.0 on macOS 14.6. Browser language order: Japanese first, then English (chrome://settings/languages). The issue was filed against 1.9.4/1.10.0; it is still present on 1.11.7.

Steps:

1. Created an index.html loading p5.js 1.11.7 from the CDN with this sketch:

```js
function setup() {
  createCanvas(400, 400);
  let value = 0;
}
```

2. Set Chrome's language priority to Japanese first, reloaded the page, opened the console.

Console output (Japanese-first):

```
TypeError: Cannot read properties of undefined (reading 'replaceAll')
p5.js translator called before translations were loaded
```

3. Control: moved English to the top of the language list, reloaded the same page.

Console output (English-first):

```
🌸 p5.js says: you have used a p5.js reserved function "value" make sure you change the function name to something else.
```

Expected: the reserved-word FES message appears regardless of browser language (translated, or English as fallback).

Actual: with a non-English language first, the TypeError above replaces the FES message entirely; with English first, the message renders. This matches the behavior the issue describes exactly.
