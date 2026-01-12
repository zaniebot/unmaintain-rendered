```yaml
number: 17516
title: Update dependency ruff to v0.11.6
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-04-21T02:05:41Z
updated_at: 2025-04-21T08:49:23Z
url: https://github.com/astral-sh/ruff/pull/17516
synced_at: 2026-01-12T15:56:02Z
```

# Update dependency ruff to v0.11.6

---

_@renovate_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.9.10` -> `==0.11.6` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.11.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.11.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.9.10/0.11.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.9.10/0.11.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.11.6`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0116)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.11.5...0.11.6)

##### Preview features

-   Avoid adding whitespace to the end of a docstring after an escaped quote ([#&#8203;17216](https://redirect.github.com/astral-sh/ruff/pull/17216))
-   \[`airflow`] Extract `AIR311` from `AIR301` rules (`AIR301`, `AIR311`) ([#&#8203;17310](https://redirect.github.com/astral-sh/ruff/pull/17310), [#&#8203;17422](https://redirect.github.com/astral-sh/ruff/pull/17422))

##### Bug fixes

-   Raise syntax error when `\` is at end of file ([#&#8203;17409](https://redirect.github.com/astral-sh/ruff/pull/17409))

### [`v0.11.5`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0115)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.11.4...0.11.5)

##### Preview features

-   \[`airflow`] Add missing `AIR302` attribute check ([#&#8203;17115](https://redirect.github.com/astral-sh/ruff/pull/17115))
-   \[`airflow`] Expand module path check to individual symbols (`AIR302`) ([#&#8203;17278](https://redirect.github.com/astral-sh/ruff/pull/17278))
-   \[`airflow`] Extract `AIR312` from `AIR302` rules (`AIR302`, `AIR312`) ([#&#8203;17152](https://redirect.github.com/astral-sh/ruff/pull/17152))
-   \[`airflow`] Update oudated `AIR301`, `AIR302` rules ([#&#8203;17123](https://redirect.github.com/astral-sh/ruff/pull/17123))
-   \[syntax-errors] Async comprehension in sync comprehension ([#&#8203;17177](https://redirect.github.com/astral-sh/ruff/pull/17177))
-   \[syntax-errors] Check annotations in annotated assignments ([#&#8203;17283](https://redirect.github.com/astral-sh/ruff/pull/17283))
-   \[syntax-errors] Extend annotation checks to `await` ([#&#8203;17282](https://redirect.github.com/astral-sh/ruff/pull/17282))

##### Bug fixes

-   \[`flake8-pie`] Avoid false positive for multiple assignment with `auto()` (`PIE796`) ([#&#8203;17274](https://redirect.github.com/astral-sh/ruff/pull/17274))

##### Rule changes

-   \[`ruff`] Fix `RUF100` to detect unused file-level `noqa` directives with specific codes ([#&#8203;17042](https://redirect.github.com/astral-sh/ruff/issues/17042)) ([#&#8203;17061](https://redirect.github.com/astral-sh/ruff/pull/17061))
-   \[`flake8-pytest-style`] Avoid false positive for legacy form of `pytest.raises` (`PT011`) ([#&#8203;17231](https://redirect.github.com/astral-sh/ruff/pull/17231))

##### Documentation

-   Fix formatting of "See Style Guide" link ([#&#8203;17272](https://redirect.github.com/astral-sh/ruff/pull/17272))

### [`v0.11.4`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0114)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.11.3...0.11.4)

##### Preview features

-   \[`ruff`] Implement `invalid-rule-code` as `RUF102` ([#&#8203;17138](https://redirect.github.com/astral-sh/ruff/pull/17138))
-   \[syntax-errors] Detect duplicate keys in `match` mapping patterns ([#&#8203;17129](https://redirect.github.com/astral-sh/ruff/pull/17129))
-   \[syntax-errors] Detect duplicate attributes in `match` class patterns ([#&#8203;17186](https://redirect.github.com/astral-sh/ruff/pull/17186))
-   \[syntax-errors] Detect invalid syntax in annotations ([#&#8203;17101](https://redirect.github.com/astral-sh/ruff/pull/17101))

##### Bug fixes

-   \[syntax-errors] Fix multiple assignment error for class fields in `match` patterns ([#&#8203;17184](https://redirect.github.com/astral-sh/ruff/pull/17184))
-   Don't skip visiting non-tuple slice in `typing.Annotated` subscripts ([#&#8203;17201](https://redirect.github.com/astral-sh/ruff/pull/17201))

### [`v0.11.3`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0113)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.11.2...0.11.3)

##### Preview features

-   \[`airflow`] Add more autofixes for `AIR302` ([#&#8203;16876](https://redirect.github.com/astral-sh/ruff/pull/16876), [#&#8203;16977](https://redirect.github.com/astral-sh/ruff/pull/16977), [#&#8203;16976](https://redirect.github.com/astral-sh/ruff/pull/16976), [#&#8203;16965](https://redirect.github.com/astral-sh/ruff/pull/16965))
-   \[`airflow`] Move `AIR301` to `AIR002` ([#&#8203;16978](https://redirect.github.com/astral-sh/ruff/pull/16978))
-   \[`airflow`] Move `AIR302` to `AIR301` and `AIR303` to `AIR302` ([#&#8203;17151](https://redirect.github.com/astral-sh/ruff/pull/17151))
-   \[`flake8-bandit`] Mark `str` and `list[str]` literals as trusted input (`S603`) ([#&#8203;17136](https://redirect.github.com/astral-sh/ruff/pull/17136))
-   \[`ruff`] Support slices in `RUF005` ([#&#8203;17078](https://redirect.github.com/astral-sh/ruff/pull/17078))
-   \[syntax-errors] Start detecting compile-time syntax errors ([#&#8203;16106](https://redirect.github.com/astral-sh/ruff/pull/16106))
-   \[syntax-errors] Duplicate type parameter names ([#&#8203;16858](https://redirect.github.com/astral-sh/ruff/pull/16858))
-   \[syntax-errors] Irrefutable `case` pattern before final case ([#&#8203;16905](https://redirect.github.com/astral-sh/ruff/pull/16905))
-   \[syntax-errors] Multiple assignments in `case` pattern ([#&#8203;16957](https://redirect.github.com/astral-sh/ruff/pull/16957))
-   \[syntax-errors] Single starred assignment target ([#&#8203;17024](https://redirect.github.com/astral-sh/ruff/pull/17024))
-   \[syntax-errors] Starred expressions in `return`, `yield`, and `for` ([#&#8203;17134](https://redirect.github.com/astral-sh/ruff/pull/17134))
-   \[syntax-errors] Store to or delete `__debug__` ([#&#8203;16984](https://redirect.github.com/astral-sh/ruff/pull/16984))

##### Bug fixes

-   Error instead of `panic!` when running Ruff from a deleted directory ([#&#8203;16903](https://redirect.github.com/astral-sh/ruff/issues/16903)) ([#&#8203;17054](https://redirect.github.com/astral-sh/ruff/pull/17054))
-   \[syntax-errors] Fix false positive for parenthesized tuple index ([#&#8203;16948](https://redirect.github.com/astral-sh/ruff/pull/16948))

##### CLI

-   Check `pyproject.toml` correctly when it is passed via stdin ([#&#8203;16971](https://redirect.github.com/astral-sh/ruff/pull/16971))

##### Configuration

-   \[`flake8-import-conventions`] Add import `numpy.typing as npt` to default `flake8-import-conventions.aliases` ([#&#8203;17133](https://redirect.github.com/astral-sh/ruff/pull/17133))

##### Documentation

-   \[`refurb`] Document why `UserDict`, `UserList`, and `UserString` are preferred over `dict`, `list`, and `str` (`FURB189`) ([#&#8203;16927](https://redirect.github.com/astral-sh/ruff/pull/16927))

### [`v0.11.2`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0112)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.11.1...0.11.2)

##### Preview features

-   \[syntax-errors] Fix false-positive syntax errors emitted for annotations on variadic parameters before Python 3.11 ([#&#8203;16878](https://redirect.github.com/astral-sh/ruff/pull/16878))

### [`v0.11.1`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0111)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.11.0...0.11.1)

##### Preview features

-   \[`airflow`] Add `chain`, `chain_linear` and `cross_downstream` for `AIR302` ([#&#8203;16647](https://redirect.github.com/astral-sh/ruff/pull/16647))
-   \[syntax-errors] Improve error message and range for pre-PEP-614 decorator syntax errors ([#&#8203;16581](https://redirect.github.com/astral-sh/ruff/pull/16581))
-   \[syntax-errors] PEP 701 f-strings before Python 3.12 ([#&#8203;16543](https://redirect.github.com/astral-sh/ruff/pull/16543))
-   \[syntax-errors] Parenthesized context managers before Python 3.9 ([#&#8203;16523](https://redirect.github.com/astral-sh/ruff/pull/16523))
-   \[syntax-errors] Star annotations before Python 3.11 ([#&#8203;16545](https://redirect.github.com/astral-sh/ruff/pull/16545))
-   \[syntax-errors] Star expression in index before Python 3.11 ([#&#8203;16544](https://redirect.github.com/astral-sh/ruff/pull/16544))
-   \[syntax-errors] Unparenthesized assignment expressions in sets and indexes ([#&#8203;16404](https://redirect.github.com/astral-sh/ruff/pull/16404))

##### Bug fixes

-   Server: Allow `FixAll` action in presence of version-specific syntax errors ([#&#8203;16848](https://redirect.github.com/astral-sh/ruff/pull/16848))
-   \[`flake8-bandit`] Allow raw strings in `suspicious-mark-safe-usage` (`S308`) [#&#8203;16702](https://redirect.github.com/astral-sh/ruff/issues/16702) ([#&#8203;16770](https://redirect.github.com/astral-sh/ruff/pull/16770))
-   \[`refurb`] Avoid panicking `unwrap` in `verbose-decimal-constructor` (`FURB157`) ([#&#8203;16777](https://redirect.github.com/astral-sh/ruff/pull/16777))
-   \[`refurb`] Fix starred expressions fix (`FURB161`) ([#&#8203;16550](https://redirect.github.com/astral-sh/ruff/pull/16550))
-   Fix `--statistics` reporting for unsafe fixes ([#&#8203;16756](https://redirect.github.com/astral-sh/ruff/pull/16756))

##### Rule changes

-   \[`flake8-executables`] Allow `uv run` in shebang line for `shebang-missing-python` (`EXE003`) ([#&#8203;16849](https://redirect.github.com/astral-sh/ruff/pull/16849),[#&#8203;16855](https://redirect.github.com/astral-sh/ruff/pull/16855))

##### CLI

-   Add `--exit-non-zero-on-format` ([#&#8203;16009](https://redirect.github.com/astral-sh/ruff/pull/16009))

##### Documentation

-   Update Ruff tutorial to avoid non-existent fix in `__init__.py` ([#&#8203;16818](https://redirect.github.com/astral-sh/ruff/pull/16818))
-   \[`flake8-gettext`] Swap `format-` and `printf-in-get-text-func-call` examples (`INT002`, `INT003`) ([#&#8203;16769](https://redirect.github.com/astral-sh/ruff/pull/16769))

### [`v0.11.0`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0110)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.10.0...0.11.0)

This is a follow-up to release 0.10.0. Because of a mistake in the release process, the `requires-python` inference changes were not included in that release. Ruff 0.11.0 now includes this change as well as the stabilization of the preview behavior for `PGH004`.

##### Breaking changes

-   **Changes to how the Python version is inferred when a `target-version` is not specified** ([#&#8203;16319](https://redirect.github.com/astral-sh/ruff/pull/16319))

    In previous versions of Ruff, you could specify your Python version with:

    -   The `target-version` option in a `ruff.toml` file or the `[tool.ruff]` section of a pyproject.toml file.
    -   The `project.requires-python` field in a `pyproject.toml` file with a `[tool.ruff]` section.

    These options worked well in most cases, and are still recommended for fine control of the Python version. However, because of the way Ruff discovers config files, `pyproject.toml` files without a `[tool.ruff]` section would be ignored, including the `requires-python` setting. Ruff would then use the default Python version (3.9 as of this writing) instead, which is surprising when you've attempted to request another version.

    In v0.10, config discovery has been updated to address this issue:

    -   If Ruff finds a `ruff.toml` file without a `target-version`, it will check
        for a `pyproject.toml` file in the same directory and respect its
        `requires-python` version, even if it does not contain a `[tool.ruff]`
        section.
    -   If Ruff finds a user-level configuration, the `requires-python` field of the closest `pyproject.toml` in a parent directory will take precedence.
    -   If there is no config file (`ruff.toml`or `pyproject.toml` with a
        `[tool.ruff]` section) in the directory of the file being checked, Ruff will
        search for the closest `pyproject.toml` in the parent directories and use its
        `requires-python` setting.

##### Stabilization

The following behaviors have been stabilized:

-   [`blanket-noqa`](https://docs.astral.sh/ruff/rules/blanket-noqa/) (`PGH004`): Also detect blanked file-level noqa comments (and not just line level comments).

##### Preview features

-   \[syntax-errors] Tuple unpacking in `for` statement iterator clause before Python 3.9 ([#&#8203;16558](https://redirect.github.com/astral-sh/ruff/pull/16558))

### [`v0.10.0`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0100)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.9.10...0.10.0)

Check out the [blog post](https://astral.sh/blog/ruff-v0.10.0) for a migration guide and overview of the changes!

##### Breaking changes

See also, the "Remapped rules" section which may result in disabled rules.

-   **Changes to how the Python version is inferred when a `target-version` is not specified** ([#&#8203;16319](https://redirect.github.com/astral-sh/ruff/pull/16319))

    Because of a mistake in the release process, the `requires-python` inference changes are not included in this release and instead shipped as part of 0.11.0.
    You can find a description of this change in the 0.11.0 section.

-   **Updated `TYPE_CHECKING` behavior** ([#&#8203;16669](https://redirect.github.com/astral-sh/ruff/pull/16669))

    Previously, Ruff only recognized typechecking blocks that tested the `typing.TYPE_CHECKING` symbol. Now, Ruff recognizes any local variable named `TYPE_CHECKING`. This release also removes support for the legacy `if 0:` and `if False:` typechecking checks. Use a local `TYPE_CHECKING` variable instead.

-   **More robust noqa parsing** ([#&#8203;16483](https://redirect.github.com/astral-sh/ruff/pull/16483))

    The syntax for both file-level and in-line suppression comments has been unified and made more robust to certain errors. In most cases, this will result in more suppression comments being read by Ruff, but there are a few instances where previously read comments will now log an error to the user instead. Please refer to the documentation on [*Error suppression*](https://docs.astral.sh/ruff/linter/#error-suppression) for the full specification.

-   **Avoid unnecessary parentheses around with statements with a single context manager and a trailing comment** ([#&#8203;14005](https://redirect.github.com/astral-sh/ruff/pull/14005))

    This change fixes a bug in the formatter where it introduced unnecessary parentheses around with statements with a single context manager and a trailing comment. This change may result in a change in formatting for some users.

-   **Bump alpine default tag to 3.21 for derived Docker images** ([#&#8203;16456](https://redirect.github.com/astral-sh/ruff/pull/16456))

    Alpine 3.21 was released in Dec 2024 and is used in the official Alpine-based Python images. Now the ruff:alpine image will use 3.21 instead of 3.20 and ruff:alpine3.20 will no longer be updated.

##### Deprecated Rules

The following rules have been deprecated:

-   [`non-pep604-isinstance`](https://docs.astral.sh/ruff/rules/non-pep604-isinstance/) (`UP038`)
-   [`suspicious-xmle-tree-usage`](https://docs.astral.sh/ruff/rules/suspicious-xmle-tree-usage/) (`S320`)

##### Remapped rules

The following rules have been remapped to new rule codes:

-   \[`unsafe-markup-use`]: `RUF035` to `S704`

##### Stabilization

The following rules have been stabilized and are no longer in preview:

-   [`batched-without-explicit-strict`](https://docs.astral.sh/ruff/rules/batched-without-explicit-strict) (`B911`)
-   [`unnecessary-dict-comprehension-for-iterable`](https://docs.astral.sh/ruff/rules/unnecessary-dict-comprehension-for-iterable) (`C420`)
-   [`datetime-min-max`](https://docs.astral.sh/ruff/rules/datetime-min-max) (`DTZ901`)
-   [`fast-api-unused-path-parameter`](https://docs.astral.sh/ruff/rules/fast-api-unused-path-parameter) (`FAST003`)
-   [`root-logger-call`](https://docs.astral.sh/ruff/rules/root-logger-call) (`LOG015`)
-   [`len-test`](https://docs.astral.sh/ruff/rules/len-test) (`PLC1802`)
-   [`shallow-copy-environ`](https://docs.astral.sh/ruff/rules/shallow-copy-environ) (`PLW1507`)
-   [`os-listdir`](https://docs.astral.sh/ruff/rules/os-listdir) (`PTH208`)
-   [`invalid-pathlib-with-suffix`](https://docs.astral.sh/ruff/rules/invalid-pathlib-with-suffix) (`PTH210`)
-   [`invalid-assert-message-literal-argument`](https://docs.astral.sh/ruff/rules/invalid-assert-message-literal-argument) (`RUF040`)
-   [`unnecessary-nested-literal`](https://docs.astral.sh/ruff/rules/unnecessary-nested-literal) (`RUF041`)
-   [`unnecessary-cast-to-int`](https://docs.astral.sh/ruff/rules/unnecessary-cast-to-int) (`RUF046`)
-   [`map-int-version-parsing`](https://docs.astral.sh/ruff/rules/map-int-version-parsing) (`RUF048`)
-   [`if-key-in-dict-del`](https://docs.astral.sh/ruff/rules/if-key-in-dict-del) (`RUF051`)
-   [`unsafe-markup-use`](https://docs.astral.sh/ruff/rules/unsafe-markup-use) (`S704`). This rule has also been renamed from `RUF035`.
-   [`split-static-string`](https://docs.astral.sh/ruff/rules/split-static-string) (`SIM905`)
-   [`runtime-cast-value`](https://docs.astral.sh/ruff/rules/runtime-cast-value) (`TC006`)
-   [`unquoted-type-alias`](https://docs.astral.sh/ruff/rules/unquoted-type-alias) (`TC007`)
-   [`non-pep646-unpack`](https://docs.astral.sh/ruff/rules/non-pep646-unpack) (`UP044`)

The following behaviors have been stabilized:

-   [`bad-staticmethod-argument`](https://docs.astral.sh/ruff/rules/bad-staticmethod-argument/) (`PLW0211`) [`invalid-first-argument-name-for-class-method`](https://docs.astral.sh/ruff/rules/invalid-first-argument-name-for-class-method/) (`N804`): `__new__` methods are now no longer flagged by `invalid-first-argument-name-for-class-method` (`N804`) but instead by `bad-staticmethod-argument` (`PLW0211`)
-   [`bad-str-strip-call`](https://docs.astral.sh/ruff/rules/bad-str-strip-call/) (`PLE1310`): The rule now applies to objects which are known to have type `str` or `bytes`.
-   [`custom-type-var-for-self`](https://docs.astral.sh/ruff/rules/custom-type-var-for-self/) (`PYI019`): More accurate detection of custom `TypeVars` replaceable by `Self`. The range of the diagnostic is now the full function header rather than just the return annotation.
-   [`invalid-argument-name`](https://docs.astral.sh/ruff/rules/invalid-argument-name/) (`N803`): Ignore argument names of functions decorated with `typing.override`
-   [`invalid-envvar-default`](https://docs.astral.sh/ruff/rules/invalid-envvar-default/) (`PLW1508`): Detect default value arguments to `os.environ.get` with invalid type.
-   [`pytest-raises-with-multiple-statements`](https://docs.astral.sh/ruff/rules/pytest-raises-with-multiple-statements/) (`PT012`) [`pytest-warns-with-multiple-statements`](https://docs.astral.sh/ruff/rules/pytest-warns-with-multiple-statements/) (`PT031`): Allow `for` statements with an empty body in `pytest.raises` and `pytest.warns` `with` statements.
-   [`redundant-open-modes`](https://docs.astral.sh/ruff/rules/redundant-open-modes/) (`UP015`): The diagnostic range is now the range of the redundant mode argument where it previously was the range of the entire open call. You may have to replace your `noqa` comments when suppressing `UP015`.
-   [`stdlib-module-shadowing`](https://docs.astral.sh/ruff/rules/stdlib-module-shadowing/) (`A005`): Changes the default value of `lint.flake8-builtins.strict-checking` from `true` to `false`.
-   [`type-none-comparison`](https://docs.astral.sh/ruff/rules/type-none-comparison/) (`FURB169`): Now also recognizes `type(expr) is type(None)` comparisons where `expr` isn't a name expression.

The following fixes or improvements to fixes have been stabilized:

-   [`repeated-equality-comparison`](https://docs.astral.sh/ruff/rules/repeated-equality-comparison/) (`PLR1714`) ([#&#8203;16685](https://redirect.github.com/astral-sh/ruff/pull/16685))
-   [`needless-bool`](https://docs.astral.sh/ruff/rules/needless-bool/) (`SIM103`) ([#&#8203;16684](https://redirect.github.com/astral-sh/ruff/pull/16684))
-   [`unused-private-type-var`](https://docs.astral.sh/ruff/rules/unused-private-type-var/) (`PYI018`) ([#&#8203;16682](https://redirect.github.com/astral-sh/ruff/pull/16682))

##### Server

-   Remove logging output for `ruff.printDebugInformation` ([#&#8203;16617](https://redirect.github.com/astral-sh/ruff/pull/16617))

##### Configuration

-   \[`flake8-builtins`] Deprecate the `builtins-` prefixed options in favor of the unprefixed options (e.g. `builtins-allowed-modules` is now deprecated in favor of `allowed-modules`) ([#&#8203;16092](https://redirect.github.com/astral-sh/ruff/pull/16092))

##### Bug fixes

-   \[flake8-bandit] Fix mixed-case hash algorithm names (S324) ([#&#8203;16552](https://redirect.github.com/astral-sh/ruff/pull/16552))

##### CLI

-   \[ruff] Fix `last_tag`/`commits_since_last_tag` for `version` command ([#&#8203;16686](https://redirect.github.com/astral-sh/ruff/pull/16686))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4yNDguNCIsInVwZGF0ZWRJblZlciI6IjM5LjI0OC40IiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-04-21 02:05_

---

_Merged by @AlexWaygood on 2025-04-21 08:49_

---

_Closed by @AlexWaygood on 2025-04-21 08:49_

---

_Branch deleted on 2025-04-21 08:49_

---
