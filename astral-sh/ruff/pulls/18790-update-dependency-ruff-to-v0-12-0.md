```yaml
number: 18790
title: Update dependency ruff to v0.12.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-06-19T09:29:23Z
updated_at: 2025-06-19T09:32:48Z
url: https://github.com/astral-sh/ruff/pull/18790
synced_at: 2026-01-12T15:56:25Z
```

# Update dependency ruff to v0.12.0

---

_@renovate_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.11.13` -> `==0.12.0` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.12.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.12.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.11.13/0.12.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.11.13/0.12.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.12.0`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0120)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.11.13...0.12.0)

Check out the [blog post](https://astral.sh/blog/ruff-v0.12.0) for a migration
guide and overview of the changes!

##### Breaking changes

-   **Detection of more syntax errors**

    Ruff now detects version-related syntax errors, such as the use of the `match`
    statement on Python versions before 3.10, and syntax errors emitted by
    CPython's compiler, such as irrefutable `match` patterns before the final
    `case` arm.

-   **New default Python version handling for syntax errors**

    Ruff will default to the *latest* supported Python version (3.13) when
    checking for the version-related syntax errors mentioned above to prevent
    false positives in projects without a Python version configured. The default
    in all other cases, like applying lint rules, is unchanged and remains at the
    minimum supported Python version (3.9).

-   **Updated f-string formatting**

    Ruff now formats multi-line f-strings with format specifiers to avoid adding a
    line break after the format specifier. This addresses a change to the Python
    grammar in version 3.13.4 that made such a line break a syntax error.

-   **`rust-toolchain.toml` is no longer included in source distributions**

    The `rust-toolchain.toml` is used to specify a higher Rust version than Ruff's
    minimum supported Rust version (MSRV) for development and building release
    artifacts. However, when present in source distributions, it would also cause
    downstream package maintainers to pull in the same Rust toolchain, even if
    their available toolchain was MSRV-compatible.

##### Removed Rules

The following rules have been removed:

-   [`suspicious-xmle-tree-usage`](https://docs.astral.sh/ruff/rules/suspicious-xmle-tree-usage/)
    (`S320`)

##### Deprecated Rules

The following rules have been deprecated:

-   [`pandas-df-variable-name`](https://docs.astral.sh/ruff/rules/pandas-df-variable-name/)

##### Stabilization

The following rules have been stabilized and are no longer in preview:

-   [`for-loop-writes`](https://docs.astral.sh/ruff/rules/for-loop-writes) (`FURB122`)
-   [`check-and-remove-from-set`](https://docs.astral.sh/ruff/rules/check-and-remove-from-set) (`FURB132`)
-   [`verbose-decimal-constructor`](https://docs.astral.sh/ruff/rules/verbose-decimal-constructor) (`FURB157`)
-   [`fromisoformat-replace-z`](https://docs.astral.sh/ruff/rules/fromisoformat-replace-z) (`FURB162`)
-   [`int-on-sliced-str`](https://docs.astral.sh/ruff/rules/int-on-sliced-str) (`FURB166`)
-   [`exc-info-outside-except-handler`](https://docs.astral.sh/ruff/rules/exc-info-outside-except-handler) (`LOG014`)
-   [`import-outside-top-level`](https://docs.astral.sh/ruff/rules/import-outside-top-level) (`PLC0415`)
-   [`unnecessary-dict-index-lookup`](https://docs.astral.sh/ruff/rules/unnecessary-dict-index-lookup) (`PLR1733`)
-   [`nan-comparison`](https://docs.astral.sh/ruff/rules/nan-comparison) (`PLW0177`)
-   [`eq-without-hash`](https://docs.astral.sh/ruff/rules/eq-without-hash) (`PLW1641`)
-   [`pytest-parameter-with-default-argument`](https://docs.astral.sh/ruff/rules/pytest-parameter-with-default-argument) (`PT028`)
-   [`pytest-warns-too-broad`](https://docs.astral.sh/ruff/rules/pytest-warns-too-broad) (`PT030`)
-   [`pytest-warns-with-multiple-statements`](https://docs.astral.sh/ruff/rules/pytest-warns-with-multiple-statements) (`PT031`)
-   [`invalid-formatter-suppression-comment`](https://docs.astral.sh/ruff/rules/invalid-formatter-suppression-comment) (`RUF028`)
-   [`dataclass-enum`](https://docs.astral.sh/ruff/rules/dataclass-enum) (`RUF049`)
-   [`class-with-mixed-type-vars`](https://docs.astral.sh/ruff/rules/class-with-mixed-type-vars) (`RUF053`)
-   [`unnecessary-round`](https://docs.astral.sh/ruff/rules/unnecessary-round) (`RUF057`)
-   [`starmap-zip`](https://docs.astral.sh/ruff/rules/starmap-zip) (`RUF058`)
-   [`non-pep604-annotation-optional`](https://docs.astral.sh/ruff/rules/non-pep604-annotation-optional) (`UP045`)
-   [`non-pep695-generic-class`](https://docs.astral.sh/ruff/rules/non-pep695-generic-class) (`UP046`)
-   [`non-pep695-generic-function`](https://docs.astral.sh/ruff/rules/non-pep695-generic-function) (`UP047`)
-   [`private-type-parameter`](https://docs.astral.sh/ruff/rules/private-type-parameter) (`UP049`)

The following behaviors have been stabilized:

-   \[`collection-literal-concatenation`] (`RUF005`) now recognizes slices, in
    addition to list literals and variables.
-   The fix for \[`readlines-in-for`] (`FURB129`) is now marked as always safe.
-   \[`if-else-block-instead-of-if-exp`] (`SIM108`) will now further simplify
    expressions to use `or` instead of an `if` expression, where possible.
-   \[`unused-noqa`] (`RUF100`) now checks for file-level `noqa` comments as well
    as inline comments.
-   \[`subprocess-without-shell-equals-true`] (`S603`) now accepts literal strings,
    as well as lists and tuples of literal strings, as trusted input.
-   \[`boolean-type-hint-positional-argument`] (`FBT001`) now applies to types that
    include `bool`, like `bool | int` or `typing.Optional[bool]`, in addition to
    plain `bool` annotations.
-   \[`non-pep604-annotation-union`] (`UP007`) has now been split into two rules.
    `UP007` now applies only to `typing.Union`, while
    \[`non-pep604-annotation-optional`] (`UP045`) checks for use of
    `typing.Optional`. `UP045` has also been stabilized in this release, but you
    may need to update existing `include`, `ignore`, or `noqa` settings to
    accommodate this change.

##### Preview features

-   \[`ruff`] Check for non-context-manager use of `pytest.raises`, `pytest.warns`, and `pytest.deprecated_call` (`RUF061`) ([#&#8203;17368](https://redirect.github.com/astral-sh/ruff/pull/17368))
-   \[syntax-errors] Raise unsupported syntax error for template strings prior to Python 3.14 ([#&#8203;18664](https://redirect.github.com/astral-sh/ruff/pull/18664))

##### Bug fixes

-   Add syntax error when conversion flag does not immediately follow exclamation mark ([#&#8203;18706](https://redirect.github.com/astral-sh/ruff/pull/18706))
-   Add trailing space around `readlines` ([#&#8203;18542](https://redirect.github.com/astral-sh/ruff/pull/18542))
-   Fix `\r` and `\r\n` handling in t- and f-string debug texts ([#&#8203;18673](https://redirect.github.com/astral-sh/ruff/pull/18673))
-   Hug closing `}` when f-string expression has a format specifier ([#&#8203;18704](https://redirect.github.com/astral-sh/ruff/pull/18704))
-   \[`flake8-pyi`] Avoid syntax error in the case of starred and keyword arguments (`PYI059`) ([#&#8203;18611](https://redirect.github.com/astral-sh/ruff/pull/18611))
-   \[`flake8-return`] Fix `RET504` autofix generating a syntax error ([#&#8203;18428](https://redirect.github.com/astral-sh/ruff/pull/18428))
-   \[`pep8-naming`] Suppress fix for `N804` and `N805` if the recommended name is already used ([#&#8203;18472](https://redirect.github.com/astral-sh/ruff/pull/18472))
-   \[`pycodestyle`] Avoid causing a syntax error in expressions spanning multiple lines (`E731`) ([#&#8203;18479](https://redirect.github.com/astral-sh/ruff/pull/18479))
-   \[`pyupgrade`] Suppress `UP008` if `super` is shadowed ([#&#8203;18688](https://redirect.github.com/astral-sh/ruff/pull/18688))
-   \[`refurb`] Parenthesize lambda and ternary expressions (`FURB122`, `FURB142`) ([#&#8203;18592](https://redirect.github.com/astral-sh/ruff/pull/18592))
-   \[`ruff`] Handle extra arguments to `deque` (`RUF037`) ([#&#8203;18614](https://redirect.github.com/astral-sh/ruff/pull/18614))
-   \[`ruff`] Preserve parentheses around `deque` in fix for `unnecessary-empty-iterable-within-deque-call` (`RUF037`) ([#&#8203;18598](https://redirect.github.com/astral-sh/ruff/pull/18598))
-   \[`ruff`] Validate arguments before offering a fix (`RUF056`) ([#&#8203;18631](https://redirect.github.com/astral-sh/ruff/pull/18631))
-   \[`ruff`] Skip fix for `RUF059` if dummy name is already bound ([#&#8203;18509](https://redirect.github.com/astral-sh/ruff/pull/18509))
-   \[`pylint`] Fix `PLW0128` to check assignment targets in square brackets and after asterisks ([#&#8203;18665](https://redirect.github.com/astral-sh/ruff/pull/18665))

##### Rule changes

-   Fix false positive on mutations in `return` statements (`B909`) ([#&#8203;18408](https://redirect.github.com/astral-sh/ruff/pull/18408))
-   Treat `ty:` comments as pragma comments ([#&#8203;18532](https://redirect.github.com/astral-sh/ruff/pull/18532))
-   \[`flake8-pyi`] Apply `custom-typevar-for-self` to string annotations (`PYI019`) ([#&#8203;18311](https://redirect.github.com/astral-sh/ruff/pull/18311))
-   \[`pyupgrade`] Don't offer a fix for `Optional[None]` (`UP007`, `UP045)` ([#&#8203;18545](https://redirect.github.com/astral-sh/ruff/pull/18545))
-   \[`pyupgrade`] Fix `super(__class__, self)` detection (`UP008`) ([#&#8203;18478](https://redirect.github.com/astral-sh/ruff/pull/18478))
-   \[`refurb`] Make the fix for `FURB163` unsafe for `log2`, `log10`, `*args`, and deleted comments ([#&#8203;18645](https://redirect.github.com/astral-sh/ruff/pull/18645))

##### Server

-   Support cancellation requests ([#&#8203;18627](https://redirect.github.com/astral-sh/ruff/pull/18627))

##### Documentation

-   Drop confusing second `*` from glob pattern example for `per-file-target-version` ([#&#8203;18709](https://redirect.github.com/astral-sh/ruff/pull/18709))
-   Update Neovim configuration examples ([#&#8203;18491](https://redirect.github.com/astral-sh/ruff/pull/18491))
-   \[`pylint`] De-emphasize `__hash__ = Parent.__hash__` (`PLW1641`) ([#&#8203;18613](https://redirect.github.com/astral-sh/ruff/pull/18613))
-   \[`refurb`] Add a note about float literal handling (`FURB157`) ([#&#8203;18615](https://redirect.github.com/astral-sh/ruff/pull/18615))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MC42MC4xIiwidXBkYXRlZEluVmVyIjoiNDAuNjAuMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-06-19 09:29_

---

_Merged by @MichaReiser on 2025-06-19 09:32_

---

_Closed by @MichaReiser on 2025-06-19 09:32_

---

_Branch deleted on 2025-06-19 09:32_

---
