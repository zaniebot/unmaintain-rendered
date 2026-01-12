```yaml
number: 9568
title: Update Rust crate miette to v7.5.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/miette-7.x-lockfile
created_at: 2024-12-02T00:12:59Z
updated_at: 2025-02-01T04:06:16Z
url: https://github.com/astral-sh/uv/pull/9568
synced_at: 2026-01-12T16:08:52Z
```

# Update Rust crate miette to v7.5.0

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [miette](https://redirect.github.com/zkat/miette) | workspace.dependencies | minor | `7.2.0` -> `7.5.0` |

---

### Release Notes

<details>
<summary>zkat/miette (miette)</summary>

### [`v7.5.0`](https://redirect.github.com/zkat/miette/blob/HEAD/CHANGELOG.md#750-2025-02-01)

##### Features

-   **graphical:** support rendering related diagnostics as nested ([#&#8203;417](https://redirect.github.com/zkat/miette/issues/417)) ([771a0751](https://redirect.github.com/zkat/miette/commit/771a07519f078b94aceb1a2d2532d786f09f350b))

##### Bug Fixes

-   **graphical:** prevent leading newline when no link/code ([#&#8203;418](https://redirect.github.com/zkat/miette/issues/418)) ([1e1938a0](https://redirect.github.com/zkat/miette/commit/1e1938a099409969c69b9f070e0fb0d13d564527))

### [`v7.4.0`](https://redirect.github.com/zkat/miette/blob/HEAD/CHANGELOG.md#740-2024-11-27)

[Compare Source](https://redirect.github.com/zkat/miette/compare/v7.3.0...v7.4.0)

##### Features

-   **graphical:** Inherit source code to causes ([#&#8203;401](https://redirect.github.com/zkat/miette/issues/401)) ([465e6b6a](https://redirect.github.com/zkat/miette/commit/465e6b6ab627f8da34baa5f46441d944fb88186e))
-   **report:** Implement `WrapError` for `Option` ([#&#8203;409](https://redirect.github.com/zkat/miette/issues/409)) ([7fae60fd](https://redirect.github.com/zkat/miette/commit/7fae60fd8462f95cf3140c6a3b9eb06cb7953405))

### [`v7.3.0`](https://redirect.github.com/zkat/miette/blob/HEAD/CHANGELOG.md#730-2024-11-26)

[Compare Source](https://redirect.github.com/zkat/miette/compare/v7.2.0...v7.3.0)

##### Features

-   **SourceSpan:** add impl From<InclusiveRange> ([#&#8203;385](https://redirect.github.com/zkat/miette/issues/385)) ([73da45b6](https://redirect.github.com/zkat/miette/commit/73da45b65c965777a00ba64aa03a247c0e5241ca))
-   **Report:** add `from_err()` method to `Report` ([#&#8203;403](https://redirect.github.com/zkat/miette/issues/403)) ([93d3bd11](https://redirect.github.com/zkat/miette/commit/93d3bd118a072c35aa761f0ec74317166ec08113))
-   **Diagnostic:** Implement `Diagnostic` for `Infallible` ([#&#8203;402](https://redirect.github.com/zkat/miette/issues/402)) ([f3fb4c1e](https://redirect.github.com/zkat/miette/commit/f3fb4c1ecd196ce389cbd71139bb7e3b35474add))

##### Performance

-   **handlers:** optimize string-buffer reallocations ([#&#8203;387](https://redirect.github.com/zkat/miette/issues/387)) ([b8dfcda4](https://redirect.github.com/zkat/miette/commit/b8dfcda4a8c10a14116ee275250356ac991dc7be))

##### Bug Fixes

-   **graphical:** fix nested error wrapping ([#&#8203;358](https://redirect.github.com/zkat/miette/issues/358)) ([3eabbceb](https://redirect.github.com/zkat/miette/commit/3eabbcebf113d1d620a6a3f98e8a455414ed3042))
-   **docs:** updated example image (fixes [#&#8203;111](https://redirect.github.com/zkat/miette/issues/111)) ([#&#8203;270](https://redirect.github.com/zkat/miette/issues/270)) ([7b42b12c](https://redirect.github.com/zkat/miette/commit/7b42b12c5f6316322ce79c59bcb9e99f5d49edb8))
-   **clippy:** Fix clippy lints in docs ([#&#8203;365](https://redirect.github.com/zkat/miette/issues/365)) ([ea4296da](https://redirect.github.com/zkat/miette/commit/ea4296dacec3b0e4762281d9d115c1bd69ecfac3))
-   **docs:** `alt` attribut for `single-line-example.png` ([#&#8203;372](https://redirect.github.com/zkat/miette/issues/372)) ([b82cc81b](https://redirect.github.com/zkat/miette/commit/b82cc81b8ea32a1cf1b4598ed5832bc8e3b0e161))
-   **color:** setting NO_COLOR should not print ansi codes for non-terminals ([#&#8203;381](https://redirect.github.com/zkat/miette/issues/381)) ([813232ba](https://redirect.github.com/zkat/miette/commit/813232ba7957ae09e4fb9d9416d821f4fd9da66d))
-   **clippy:** fix Rust v1.78.0 clippy warnings ([#&#8203;382](https://redirect.github.com/zkat/miette/issues/382)) ([e1026f75](https://redirect.github.com/zkat/miette/commit/e1026f75e0a5d19bbc8e468cb3f5292074543a97))
-   **perf:** mark error constructors cold ([#&#8203;378](https://redirect.github.com/zkat/miette/issues/378)) ([9bbcf3c6](https://redirect.github.com/zkat/miette/commit/9bbcf3c6017fa3455a7db714879816c1cfc511fd))
-   **handlers:** Disable textwrap::smawk feature ([#&#8203;379](https://redirect.github.com/zkat/miette/issues/379)) ([edfdcb52](https://redirect.github.com/zkat/miette/commit/edfdcb525ee30fc54747460ada621f13f0ed1996))
-   **graphical:** Format entire link instead of just name ([#&#8203;389](https://redirect.github.com/zkat/miette/issues/389)) ([bf5aa374](https://redirect.github.com/zkat/miette/commit/bf5aa3742fd664be3c93160b9c28c145b1ed8bc9))
-   **clippy:** fix `clippy::doc_lazy_continuation` lints ([#&#8203;395](https://redirect.github.com/zkat/miette/issues/395)) ([15beec43](https://redirect.github.com/zkat/miette/commit/15beec43303180b811d4c04d1a78775feb9b0905))
-   **graphical:** Handle invalid UTF-8 in source code ([#&#8203;393](https://redirect.github.com/zkat/miette/issues/393)) ([d6b45585](https://redirect.github.com/zkat/miette/commit/d6b4558502e82fa74e030ccb3c8040656590d7eb))
-   **features:** Use `dep:` syntax for dependencies in features. ([#&#8203;394](https://redirect.github.com/zkat/miette/issues/394)) ([789a04e3](https://redirect.github.com/zkat/miette/commit/789a04e30d041179b373b4eb8b340456534a3f0e))
-   **clippy:** Fix `needless_return` lint. ([#&#8203;405](https://redirect.github.com/zkat/miette/issues/405)) ([5f441d01](https://redirect.github.com/zkat/miette/commit/5f441d011560a091fe5d6a6cdb05f09acf622d36))

##### Documentation

-   **examples:** add serde_json integration example ([#&#8203;407](https://redirect.github.com/zkat/miette/issues/407)) ([2902a233](https://redirect.github.com/zkat/miette/commit/2902a2337c2e36a5d8e0e54b007d6100cca0c9ff))

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

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xOS4wIiwidXBkYXRlZEluVmVyIjoiMzkuMTQ1LjAiLCJ0YXJnZXRCcmFuY2giOiJtYWluIiwibGFiZWxzIjpbImludGVybmFsIl19-->


---

_Label `internal` added by @renovate[bot] on 2024-12-02 00:13_

---

_Comment by @charliermarsh on 2024-12-02 01:08_

It looks like there's a newline where there didn't used to be.

---

_Comment by @renovate[bot] on 2025-02-01 03:54_

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
error: failed to load manifest for workspace member `/tmp/renovate/repos/github/astral-sh/uv/crates/uv`
referenced by workspace at `/tmp/renovate/repos/github/astral-sh/uv/Cargo.toml`

Caused by:
  failed to load manifest for dependency `uv-auth`

Caused by:
  failed to parse manifest at `/tmp/renovate/repos/github/astral-sh/uv/crates/uv-auth/Cargo.toml`

Caused by:
  error inheriting `percent-encoding` from workspace root manifest's `workspace.dependencies.percent-encoding`

Caused by:
  `dependency.percent-encoding` was not found in `workspace.dependencies`

```



---

_Renamed from "Update Rust crate miette to v7.4.0" to "Update Rust crate miette to v7.5.0" by @renovate[bot] on 2025-02-01 03:54_

---

_Merged by @charliermarsh on 2025-02-01 04:06_

---

_Closed by @charliermarsh on 2025-02-01 04:06_

---

_Branch deleted on 2025-02-01 04:06_

---
