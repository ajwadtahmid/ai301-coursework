# Eval package: pkg-19

- source: vuejs/core#15205
- captured: 2026-08-17
- calibration: false

## Repo facts (captured 2026-08-17)

- repo: vuejs/core (54211 stars, archived: no)
- description: Vue.js is a progressive, incrementally-adoptable JavaScript framework for building UI on the web.
- latest release: v3.5.41 (2026-08-05)
- bug reports: template requires the version, a minimal reproduction link (SFC playground or repo), steps to reproduce, what is expected, and what is actually happening
- contribution policy (.github/contributing.md): standard contribution guide; no stated AI policy

## Issue

### @vue/compiler-sfc: Partly unscoped styling with selector list & :deep & nesting (#15205)

opened by stellar7073 (NONE) on 2026-08-03, state open, labels: scope: sfc, has workaround, p3-minor-bug

Version 3.6.0-rc.2, with an SFC playground reproduction link. When a
`<style scoped>` rule has (1) a comma-separated selector list, where
(2) one member uses `:deep()`, and (3) the rule contains nested child
rules, the scope id (`[data-v-xxx]`) is not injected into the
non-`:deep()` selectors in the list. Those selectors become unscoped
and leak globally.

Trigger example:

```css
.a,
.b :deep(.c) {
  color: red;
  > span { color: blue; }
}
```

Expected: `.a[data-v-xxx]` in the output. Actually happening: `.a`
stays unscoped while `.b[data-v-xxx] .c` is handled.

## Thread highlights (2 comments total)

- 2026-08-04 edison1105 (MEMBER): spells out the expected scoped-CSS output semantics for normal nested rules and for deep rules, settling what correct output looks like
- 2026-08-11 ValentinYoushkevich (NONE): with the member's spec as reference, reports that the currently open fix PR does not satisfy it, with `compileStyle` outputs for the four selector shapes

## Candidate claim comment

Hello sir! Great project, I love Vue and use it every day. I am very
interested in contributing to this amazing repository and this issue
looks like a good one for me. Kindly assign it to me, I will fix it
within 2 days guaranteed. I have attached my reproduction below,
please check it and keep this issue reserved for me. Looking forward
to my first of many contributions here, thank you so much!

## Candidate repro report

Environment: @vue/compiler-sfc 3.6.0-rc.2 via the official SFC
playground (Chrome 139, macOS 14.6).

Steps: opened the issue's playground link, then re-created it from
scratch in a fresh playground to rule out link staleness: a component
with the issue's exact scoped style block (selector list, one `:deep()`
member, one nested `> span` rule).

Produced CSS (from the playground's CSS output pane):

```css
.a,
.b[data-v-7ba5bd90] .c {
& {
  color: red;
}
> span { color: blue;
}
}
```

Expected: `.a[data-v-7ba5bd90]` as the first selector, per the
member's comment on correct scoped output.

Actual: `.a` has no scope attribute (shown above), so it applies
globally; a sibling component with a plain `.a` class picks up the
red color, which I confirmed by adding one next to the test component
in the same playground.
