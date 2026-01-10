```yaml
number: 14711
title: Update Rust crate annotate-snippets to 0.11.0
type: pull_request
state: closed
author: renovate
labels:
  - internal
assignees: []
base: main
head: renovate/annotate-snippets-0.x
created_at: 2024-12-02T00:48:17Z
updated_at: 2024-12-02T05:06:44Z
url: https://github.com/astral-sh/ruff/pull/14711
synced_at: 2026-01-10T20:42:27Z
```

# Update Rust crate annotate-snippets to 0.11.0

---

_Pull request opened by @renovate on 2024-12-02 00:48_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [annotate-snippets](https://redirect.github.com/rust-lang/annotate-snippets-rs) | workspace.dependencies | minor | `0.9.2` -> `0.11.0` |

---

### Release Notes

<details>
<summary>rust-lang/annotate-snippets-rs (annotate-snippets)</summary>

### [`v0.11.4`](https://redirect.github.com/rust-lang/annotate-snippets-rs/blob/HEAD/CHANGELOG.md#0114---2024-06-15)

[Compare Source](https://redirect.github.com/rust-lang/annotate-snippets-rs/compare/0.11.3...0.11.4)

##### Fixes

-   Annotations for `\r\n` are now correctly handled [#&#8203;131](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/131)

### [`v0.11.3`](https://redirect.github.com/rust-lang/annotate-snippets-rs/blob/HEAD/CHANGELOG.md#0113---2024-06-06)

[Compare Source](https://redirect.github.com/rust-lang/annotate-snippets-rs/compare/0.11.2...0.11.3)

##### Fixes

-   Dropped MSRV to 1.65

### [`v0.11.2`](https://redirect.github.com/rust-lang/annotate-snippets-rs/blob/HEAD/CHANGELOG.md#0112---2024-04-27)

[Compare Source](https://redirect.github.com/rust-lang/annotate-snippets-rs/compare/0.11.1...0.11.2)

##### Added

-   All public types now implement `Debug` [#&#8203;119](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/119)

### [`v0.11.1`](https://redirect.github.com/rust-lang/annotate-snippets-rs/blob/HEAD/CHANGELOG.md#0111---2024-03-21)

[Compare Source](https://redirect.github.com/rust-lang/annotate-snippets-rs/compare/0.11.0...0.11.1)

##### Fixes

-   Switch `fold` to use rustc's logic: always show first and last line of folded section and detect if its worth folding
-   When `fold`ing the start of a `source`, don't show anything, like we do for the end of the `source`
-   Render an underline for an empty span on `Annotation`s

### [`v0.11.0`](https://redirect.github.com/rust-lang/annotate-snippets-rs/blob/HEAD/CHANGELOG.md#0110---2024-03-15)

[Compare Source](https://redirect.github.com/rust-lang/annotate-snippets-rs/compare/0.10.2...0.11.0)

##### Breaking Changes

-   Switched from char spans to byte spans [#&#8203;90](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/90/commits/b65b8cabcd34da9fed88490a7a1cd8085777706a)
-   Renamed `AnnotationType` to `Level` [#&#8203;94](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/94/commits/b49f9471d920c7f561fa61970039b0ba44e448ac)
-   Renamed `SourceAnnotation` to `Annotation` [#&#8203;94](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/94/commits/bbf9c5fe27e83652433151cbfc7d6cafc02a8c47)
-   Renamed `Snippet` to `Message` [#&#8203;94](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/94/commits/105da760b6e1bd4cfce4c642ac679ecf6011f511)
-   Renamed `Slice` to `Snippet` [#&#8203;94](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/94/commits/1c18950300cf8b93d92d89e9797ed0bae02c0a37)
-   `Message`, `Snippet`, `Annotation` and `Level` can only be built with a builder pattern [#&#8203;91](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/91) and [#&#8203;94](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/94)
-   `Annotation` labels are now optional [#&#8203;94](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/94/commits/c821084068a1acd2688b6c8d0b3423e143d359e2)
-   `Annotation` now takes in `Range<usize>` instead of `(usize, usize)` [#&#8203;90](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/90/commits/c3bd0c3a63f983f5f2b4793a099972b1f6e97a9f)
-   `Margin` is now an internal detail, only `term_width` is exposed [#&#8203;105](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/105)
-   `footer` was generalized to be a `Message` [#&#8203;98](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/98)

##### Added

-   `term_width` was added to `Renderer` to control the rendering width [#&#8203;105](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/105)
    -   defaults to 140 when not set

##### Fixed

-   `Margin`s are now calculated per `Snippet`, rather than for the entire `Message` [#&#8203;105](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/105)
-   `Annotation`s can be created without labels

##### Features

-   `footer` was expanded to allow annotating sources by accepting `Message` [#&#8203;98](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/98)

### [`v0.10.2`](https://redirect.github.com/rust-lang/annotate-snippets-rs/blob/HEAD/CHANGELOG.md#0102---2024-02-29)

[Compare Source](https://redirect.github.com/rust-lang/annotate-snippets-rs/compare/0.10.1...0.10.2)

##### Added

-   Added `testing-colors` feature to remove platform-specific colors when testing
    [#&#8203;82](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/82)

### [`v0.10.1`](https://redirect.github.com/rust-lang/annotate-snippets-rs/blob/HEAD/CHANGELOG.md#0101---2024-01-04)

[Compare Source](https://redirect.github.com/rust-lang/annotate-snippets-rs/compare/0.10.0...0.10.1)

##### Fixed

-   Match `rustc`'s colors [#&#8203;73](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/73)
-   Allow highlighting one past the end of `source` [#&#8203;74](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/74)

##### Compatibility

-   Set the minimum supported Rust version to `1.73.0` [#&#8203;71](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/71)

### [`v0.10.0`](https://redirect.github.com/rust-lang/annotate-snippets-rs/blob/HEAD/CHANGELOG.md#0100---December-12-2023)

[Compare Source](https://redirect.github.com/rust-lang/annotate-snippets-rs/compare/0.9.2...0.10.0)

##### Added

-   `Renderer` is now used for displaying a `Snippet` [#&#8203;67](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/67/commits/9076cbf66336e5137b47dc7a52df2999b6c82598)
    -   `Renderer` also controls the color scheme and formatting of the snippet

##### Changed

-   Moved everything in the `snippet` to be in the crate root [#&#8203;67](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/67/commits/a1007ddf2fc6f76e960a4fc01207228e64e9fae7)

##### Breaking Changes

-   `Renderer` now controls the color scheme and formatting of `Snippet`s [#&#8203;67](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/67/commits/d0c65b26493d60f86a82c5919ef736b35808c23a)
-   Removed the `Style` and `Stylesheet` traits, as color is controlled by `Renderer` [#&#8203;67](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/67/commits/4affdfb50ea0670d85e52737c082c03f89ae8ada)
-   Replaced [`yansi-term`](https://crates.io/crates/yansi-term) with [`anstyle`](https://crates.io/crates/anstyle) [#&#8203;67](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/67/commits/dfd4e87d6f31ec50d29af26d7310cff5e66ca978)
    -   `anstyle` is designed primarily to exist in public APIs for interoperability
    -   `anstyle` is re-exported under `annotate_snippets::renderer`
-   Removed the `color` feature in favor of `Renderer::plain()` [#&#8203;67](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/67/commits/dfd4e87d6f31ec50d29af26d7310cff5e66ca978)
-   Moved `Margin` to `renderer` module [#&#8203;67](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/67/commits/79f657ea252c3c0ce55fa69894ee520f8820b4bf)
-   Made the `display_list` module private [#&#8203;67](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/67/commits/da45f4858af3ec4c0d792ecc40225e27fdd2bac8)

##### Compatibility

-   Changed the edition to `2021` [#&#8203;61](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/61)
-   Set the minimum supported Rust version to `1.70.0` [#&#8203;61](https://redirect.github.com/rust-lang/annotate-snippets-rs/pull/61)

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

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xOS4wIiwidXBkYXRlZEluVmVyIjoiMzkuMTkuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2024-12-02 00:48_

---

_Comment by @renovate[bot] on 2024-12-02 00:48_

### ⚠️ Artifact update problem

Renovate failed to update an artifact related to this branch. You probably do not want to merge this PR as-is.

♻ Renovate will retry this branch, including artifacts, only when one of the following happens:

 - any of the package files in this branch needs updating, or 
 - the branch becomes conflicted, or
 - you click the rebase/retry checkbox if found above, or
 - you rename this PR's title to start with "rebase!" to trigger it manually

The artifact failure details are included below:

##### File name: Cargo.lock

```
Command failed: cargo update --config net.git-fetch-with-cli=true --manifest-path Cargo.toml --workspace
    Updating crates.io index
error: failed to select a version for `annotate-snippets`.
    ... required by package `ruff_python_parser v0.0.0 (/tmp/renovate/repos/github/astral-sh/ruff/crates/ruff_python_parser)`
    ... which satisfies path dependency `ruff_python_parser` (locked to 0.0.0) of package `ruff_benchmark v0.0.0 (/tmp/renovate/repos/github/astral-sh/ruff/crates/ruff_benchmark)`
versions that meet the requirements `^0.11.0` are: 0.11.4, 0.11.3, 0.11.2, 0.11.1, 0.11.0

the package `ruff_python_parser` depends on `annotate-snippets`, with features: `color` but `annotate-snippets` does not have these features.


failed to select a version for `annotate-snippets` which could resolve this conflict

```



---

_Closed by @dhruvmanila on 2024-12-02 05:06_

---

_Comment by @renovate[bot] on 2024-12-02 05:06_

### Renovate Ignore Notification

Because you closed this PR without merging, Renovate will ignore this update (`0.11.0`). You will get a PR once a newer version is released. To ignore this dependency forever, add it to the `ignoreDeps` array of your Renovate config.

If you accidentally closed this PR, or if you changed your mind: rename this PR to get a fresh replacement PR.

---

_Branch deleted on 2024-12-02 05:06_

---
