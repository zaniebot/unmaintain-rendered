```yaml
number: 10606
title: Update dependency tailwindcss to v3.4.1 - autoclosed
type: pull_request
state: closed
author: renovate
labels:
  - internal
assignees: []
base: main
head: renovate/tailwindcss-3.x-lockfile
created_at: 2024-03-26T09:59:43Z
updated_at: 2024-03-26T10:40:40Z
url: https://github.com/astral-sh/ruff/pull/10606
synced_at: 2026-01-12T15:55:32Z
```

# Update dependency tailwindcss to v3.4.1 - autoclosed

---

_@renovate_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [tailwindcss](https://tailwindcss.com) ([source](https://togithub.com/tailwindlabs/tailwindcss)) | [`3.3.3` -> `3.4.1`](https://renovatebot.com/diffs/npm/tailwindcss/3.3.3/3.4.1) | [![age](https://developer.mend.io/api/mc/badges/age/npm/tailwindcss/3.4.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/tailwindcss/3.4.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/tailwindcss/3.3.3/3.4.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/tailwindcss/3.3.3/3.4.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

### Release Notes

<details>
<summary>tailwindlabs/tailwindcss (tailwindcss)</summary>

### [`v3.4.1`](https://togithub.com/tailwindlabs/tailwindcss/releases/tag/v3.4.1)

[Compare Source](https://togithub.com/tailwindlabs/tailwindcss/compare/v3.4.0...v3.4.1)

##### Fixed

-   Don't remove keyframe stops when using important utilities ([#&#8203;12639](https://togithub.com/tailwindlabs/tailwindcss/pull/12639))
-   Don't add spaces to gradients and grid track names when followed by `calc()` ([#&#8203;12704](https://togithub.com/tailwindlabs/tailwindcss/pull/12704))
-   Restore old behavior for `class` dark mode strategy ([#&#8203;12717](https://togithub.com/tailwindlabs/tailwindcss/pull/12717))

##### Added

-   Add new `selector` and `variant` strategies for dark mode ([#&#8203;12717](https://togithub.com/tailwindlabs/tailwindcss/pull/12717))

##### Changed

-   Support `rtl` and `ltr` variants on same element as `dir` attribute ([#&#8203;12717](https://togithub.com/tailwindlabs/tailwindcss/pull/12717))

### [`v3.4.0`](https://togithub.com/tailwindlabs/tailwindcss/blob/HEAD/CHANGELOG.md#340---2023-12-19)

[Compare Source](https://togithub.com/tailwindlabs/tailwindcss/compare/v3.3.7...v3.4.0)

##### Added

-   Add `svh`, `lvh`, and `dvh` values to default `height`/`min-height`/`max-height` theme ([#&#8203;11317](https://togithub.com/tailwindlabs/tailwindcss/pull/11317))
-   Add `has-*` variants for `:has(...)` pseudo-class ([#&#8203;11318](https://togithub.com/tailwindlabs/tailwindcss/pull/11318))
-   Add `text-wrap` utilities including `text-balance` and `text-pretty` ([#&#8203;11320](https://togithub.com/tailwindlabs/tailwindcss/pull/11320), [#&#8203;12031](https://togithub.com/tailwindlabs/tailwindcss/pull/12031))
-   Extend default `opacity` scale to include all steps of 5 ([#&#8203;11832](https://togithub.com/tailwindlabs/tailwindcss/pull/11832))
-   Update Preflight `html` styles to include shadow DOM `:host` pseudo-class ([#&#8203;11200](https://togithub.com/tailwindlabs/tailwindcss/pull/11200))
-   Increase default values for `grid-rows-*` utilities from 1–6 to 1–12 ([#&#8203;12180](https://togithub.com/tailwindlabs/tailwindcss/pull/12180))
-   Add `size-*` utilities ([#&#8203;12287](https://togithub.com/tailwindlabs/tailwindcss/pull/12287))
-   Add utilities for CSS subgrid ([#&#8203;12298](https://togithub.com/tailwindlabs/tailwindcss/pull/12298))
-   Add spacing scale to `min-w-*`, `min-h-*`, and `max-w-*` utilities ([#&#8203;12300](https://togithub.com/tailwindlabs/tailwindcss/pull/12300))
-   Add `forced-color-adjust` utilities ([#&#8203;11931](https://togithub.com/tailwindlabs/tailwindcss/pull/11931))
-   Add `forced-colors` variant ([#&#8203;11694](https://togithub.com/tailwindlabs/tailwindcss/pull/11694), [#&#8203;12582](https://togithub.com/tailwindlabs/tailwindcss/pull/12582))
-   Add `appearance-auto` utility ([#&#8203;12404](https://togithub.com/tailwindlabs/tailwindcss/pull/12404))
-   Add logical property values for `float` and `clear` utilities ([#&#8203;12480](https://togithub.com/tailwindlabs/tailwindcss/pull/12480))
-   Add `*` variant for targeting direct children ([#&#8203;12551](https://togithub.com/tailwindlabs/tailwindcss/pull/12551))

##### Changed

-   Simplify the `sans` font-family stack ([#&#8203;11748](https://togithub.com/tailwindlabs/tailwindcss/pull/11748))
-   Disable the tap highlight overlay on iOS ([#&#8203;12299](https://togithub.com/tailwindlabs/tailwindcss/pull/12299))
-   Improve relative precedence of `rtl`, `ltr`, `forced-colors`, and `dark` variants ([#&#8203;12584](https://togithub.com/tailwindlabs/tailwindcss/pull/12584))

### [`v3.3.7`](https://togithub.com/tailwindlabs/tailwindcss/releases/tag/v3.3.7)

[Compare Source](https://togithub.com/tailwindlabs/tailwindcss/compare/v3.3.6...v3.3.7)

##### Fixed

-   Fix support for container query utilities with arbitrary values ([#&#8203;12534](https://togithub.com/tailwindlabs/tailwindcss/pull/12534))
-   Fix custom config loading in Standalone CLI ([#&#8203;12616](https://togithub.com/tailwindlabs/tailwindcss/pull/12616))

### [`v3.3.6`](https://togithub.com/tailwindlabs/tailwindcss/blob/HEAD/CHANGELOG.md#336---2023-12-04)

[Compare Source](https://togithub.com/tailwindlabs/tailwindcss/compare/v3.3.5...v3.3.6)

##### Fixed

-   Improve types for `resolveConfig` ([#&#8203;12272](https://togithub.com/tailwindlabs/tailwindcss/pull/12272))
-   Don’t add spaces to negative numbers following a comma ([#&#8203;12324](https://togithub.com/tailwindlabs/tailwindcss/pull/12324))
-   Don't emit `@config` in CSS when watching via the CLI ([#&#8203;12327](https://togithub.com/tailwindlabs/tailwindcss/pull/12327))
-   Ensure configured `font-feature-settings` for `mono` are included in Preflight ([#&#8203;12342](https://togithub.com/tailwindlabs/tailwindcss/pull/12342))
-   Improve candidate detection in minified JS arrays (without spaces) ([#&#8203;12396](https://togithub.com/tailwindlabs/tailwindcss/pull/12396))
-   Don't crash when given applying a variant to a negated version of a simple utility ([#&#8203;12514](https://togithub.com/tailwindlabs/tailwindcss/pull/12514))
-   Fix support for slashes in arbitrary modifiers ([#&#8203;12515](https://togithub.com/tailwindlabs/tailwindcss/pull/12515))
-   Fix source maps of variant utilities that come from an `@layer` rule ([#&#8203;12508](https://togithub.com/tailwindlabs/tailwindcss/pull/12508))
-   Fix loading of built-in plugins when using an ESM or TypeScript config with the Standalone CLI ([#&#8203;12506](https://togithub.com/tailwindlabs/tailwindcss/pull/12506))

### [`v3.3.5`](https://togithub.com/tailwindlabs/tailwindcss/blob/HEAD/CHANGELOG.md#335---2023-10-25)

[Compare Source](https://togithub.com/tailwindlabs/tailwindcss/compare/v3.3.4...v3.3.5)

##### Fixed

-   Fix incorrect spaces around `-` in `calc()` expression ([#&#8203;12283](https://togithub.com/tailwindlabs/tailwindcss/pull/12283))

### [`v3.3.4`](https://togithub.com/tailwindlabs/tailwindcss/blob/HEAD/CHANGELOG.md#334---2023-10-24)

[Compare Source](https://togithub.com/tailwindlabs/tailwindcss/compare/v3.3.3...v3.3.4)

##### Fixed

-   Improve normalisation of `calc()`-like functions ([#&#8203;11686](https://togithub.com/tailwindlabs/tailwindcss/pull/11686))
-   Skip `calc()` normalisation in nested `theme()` calls ([#&#8203;11705](https://togithub.com/tailwindlabs/tailwindcss/pull/11705))
-   Fix incorrectly generated CSS when using square brackets inside arbitrary properties ([#&#8203;11709](https://togithub.com/tailwindlabs/tailwindcss/pull/11709))
-   Make `content` optional for presets in TypeScript types ([#&#8203;11730](https://togithub.com/tailwindlabs/tailwindcss/pull/11730))
-   Handle variable colors that have variable fallback values ([#&#8203;12049](https://togithub.com/tailwindlabs/tailwindcss/pull/12049))
-   Batch reading content files to prevent `too many open files` error ([#&#8203;12079](https://togithub.com/tailwindlabs/tailwindcss/pull/12079))
-   Skip over classes inside `:not(…)` when nested in an at-rule ([#&#8203;12105](https://togithub.com/tailwindlabs/tailwindcss/pull/12105))
-   Update types to work with `Node16` module resolution ([#&#8203;12097](https://togithub.com/tailwindlabs/tailwindcss/pull/12097))
-   Don’t crash when important and parent selectors are equal in `@apply` ([#&#8203;12112](https://togithub.com/tailwindlabs/tailwindcss/pull/12112))
-   Eliminate irrelevant rules when applying variants ([#&#8203;12113](https://togithub.com/tailwindlabs/tailwindcss/pull/12113))
-   Improve RegEx parser, reduce possibilities as the key for arbitrary properties ([#&#8203;12121](https://togithub.com/tailwindlabs/tailwindcss/pull/12121))
-   Fix sorting of utilities that share multiple candidates ([#&#8203;12173](https://togithub.com/tailwindlabs/tailwindcss/pull/12173))
-   Ensure variants with arbitrary values and a modifier are correctly matched in the RegEx based parser ([#&#8203;12179](https://togithub.com/tailwindlabs/tailwindcss/pull/12179))
-   Fix crash when watching renamed files on FreeBSD ([#&#8203;12193](https://togithub.com/tailwindlabs/tailwindcss/pull/12193))
-   Allow plugins from a parent document to be used in an iframe ([#&#8203;12208](https://togithub.com/tailwindlabs/tailwindcss/pull/12208))
-   Add types for `tailwindcss/nesting` ([#&#8203;12269](https://togithub.com/tailwindlabs/tailwindcss/pull/12269))
-   Bump `jiti`, `fast-glob`, and `browserlist` dependencies ([#&#8203;11550](https://togithub.com/tailwindlabs/tailwindcss/pull/11550))
-   Improve automatic `var` injection for properties that accept a `<dashed-ident>` ([#&#8203;12236](https://togithub.com/tailwindlabs/tailwindcss/pull/12236))

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR has been generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View repository job log [here](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzNy4yNjkuMiIsInVwZGF0ZWRJblZlciI6IjM3LjI2OS4yIiwidGFyZ2V0QnJhbmNoIjoibWFpbiJ9-->


---

_Label `internal` added by @renovate[bot] on 2024-03-26 09:59_

---

_Renamed from "Update dependency tailwindcss to v3.4.1" to "Update dependency tailwindcss to v3.4.1 - autoclosed" by @renovate[bot] on 2024-03-26 10:40_

---

_Closed by @renovate[bot] on 2024-03-26 10:40_

---

_Branch deleted on 2024-03-26 10:40_

---
