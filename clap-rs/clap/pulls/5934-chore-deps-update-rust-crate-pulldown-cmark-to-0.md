```yaml
number: 5934
title: "chore(deps): Update Rust crate pulldown-cmark to 0.13.0"
type: pull_request
state: merged
author: renovate
labels: []
assignees: []
merged: true
base: master
head: renovate/pulldown-cmark-0.x
created_at: 2025-03-01T02:07:12Z
updated_at: 2025-03-03T17:01:17Z
url: https://github.com/clap-rs/clap/pull/5934
synced_at: 2026-01-12T16:14:17Z
```

# chore(deps): Update Rust crate pulldown-cmark to 0.13.0

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [pulldown-cmark](https://redirect.github.com/raphlinus/pulldown-cmark) | dependencies | minor | `0.12.2` -> `0.13.0` |

---

### Release Notes

<details>
<summary>raphlinus/pulldown-cmark (pulldown-cmark)</summary>

### [`v0.13.0`](https://redirect.github.com/pulldown-cmark/pulldown-cmark/releases/tag/v0.13.0)

[Compare Source](https://redirect.github.com/raphlinus/pulldown-cmark/compare/v0.12.2...v0.13.0)

#### Breaking Changes

-   super and sub script support by [@&#8203;jim-taylor-business](https://redirect.github.com/jim-taylor-business) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/966](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/966)
-   Implement extension WikiLinks; `Options::ENABLE_WIKILINKS` by [@&#8203;frostu8](https://redirect.github.com/frostu8) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/991](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/991)

#### New Features

-   feat: add `-D` CLI option to enable definition lists by [@&#8203;ytmimi](https://redirect.github.com/ytmimi) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/972](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/972)

#### Bug Fixes and Code Enhancements

-   Safer definition lists implementation by [@&#8203;mondeja](https://redirect.github.com/mondeja) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/974](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/974)
-   Factor duplicate code out of parsers by [@&#8203;notriddle](https://redirect.github.com/notriddle) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/976](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/976)
-   Stop using string slicing for math where bytes will do by [@&#8203;notriddle](https://redirect.github.com/notriddle) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/977](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/977)
-   Make indent calc for definition lists match commonmark-hs closer by [@&#8203;notriddle](https://redirect.github.com/notriddle) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/978](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/978)
-   Ensure "parse" fuzz target covers all options by [@&#8203;ollpu](https://redirect.github.com/ollpu) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/980](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/980)
-   Change subscript CLI flag to -B by [@&#8203;ollpu](https://redirect.github.com/ollpu) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/993](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/993)
-   Fix OOB access due to erroneous shift in process_mask by [@&#8203;ollpu](https://redirect.github.com/ollpu) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/990](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/990)
-   Use slice patterns for `unescape` by [@&#8203;notriddle](https://redirect.github.com/notriddle) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/996](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/996)
-   Use slice patterns for `scan_eol` by [@&#8203;notriddle](https://redirect.github.com/notriddle) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/998](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/998)
-   Stop using scan_ch when get will do by [@&#8203;notriddle](https://redirect.github.com/notriddle) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/1003](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/1003)
-   Fix panic when symbols are present in wikilink before pipe by [@&#8203;frostu8](https://redirect.github.com/frostu8) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/1004](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/1004)
-   Added a WASM build step to github actions [#&#8203;1005](https://redirect.github.com/raphlinus/pulldown-cmark/issues/1005) by [@&#8203;rimutaka](https://redirect.github.com/rimutaka) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/1006](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/1006)
-   Use an explicit node for tight paragraphs by [@&#8203;notriddle](https://redirect.github.com/notriddle) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/1015](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/1015)
-   Fix tasklist parsing bugs by [@&#8203;notriddle](https://redirect.github.com/notriddle) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/1017](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/1017)
-   Prevent definition list defs from interrupting non-paragraphs by [@&#8203;notriddle](https://redirect.github.com/notriddle) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/1018](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/1018)

#### Docs

-   Add basic skeleton for developer docs by [@&#8203;systemsoverload](https://redirect.github.com/systemsoverload) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/988](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/988)
-   docs: Added a doc-comment for ENABLE_SMART_PUNCTUATION option. by [@&#8203;rimutaka](https://redirect.github.com/rimutaka) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/1007](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/1007)
-   Document more Events and Tags by [@&#8203;ModProg](https://redirect.github.com/ModProg) in [https://github.com/pulldown-cmark/pulldown-cmark/pull/1010](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/1010)

#### New Contributors

-   [@&#8203;ytmimi](https://redirect.github.com/ytmimi) made their first contribution in [https://github.com/pulldown-cmark/pulldown-cmark/pull/972](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/972)
-   [@&#8203;mondeja](https://redirect.github.com/mondeja) made their first contribution in [https://github.com/pulldown-cmark/pulldown-cmark/pull/974](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/974)
-   [@&#8203;jim-taylor-business](https://redirect.github.com/jim-taylor-business) made their first contribution in [https://github.com/pulldown-cmark/pulldown-cmark/pull/966](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/966)
-   [@&#8203;systemsoverload](https://redirect.github.com/systemsoverload) made their first contribution in [https://github.com/pulldown-cmark/pulldown-cmark/pull/988](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/988)
-   [@&#8203;frostu8](https://redirect.github.com/frostu8) made their first contribution in [https://github.com/pulldown-cmark/pulldown-cmark/pull/991](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/991)
-   [@&#8203;rimutaka](https://redirect.github.com/rimutaka) made their first contribution in [https://github.com/pulldown-cmark/pulldown-cmark/pull/1006](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/1006)
-   [@&#8203;ModProg](https://redirect.github.com/ModProg) made their first contribution in [https://github.com/pulldown-cmark/pulldown-cmark/pull/1010](https://redirect.github.com/pulldown-cmark/pulldown-cmark/pull/1010)

**Full Changelog**: https://github.com/pulldown-cmark/pulldown-cmark/compare/v0.12.2...v0.13.0

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 5am on the first day of the month" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/clap-rs/clap).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xNzYuMiIsInVwZGF0ZWRJblZlciI6IjM5LjE3Ni4yIiwidGFyZ2V0QnJhbmNoIjoibWFzdGVyIiwibGFiZWxzIjpbXX0=-->


---
