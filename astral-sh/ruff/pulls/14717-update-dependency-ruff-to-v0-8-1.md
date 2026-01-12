```yaml
number: 14717
title: Update dependency ruff to v0.8.1
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2024-12-02T01:06:23Z
updated_at: 2024-12-02T01:13:14Z
url: https://github.com/astral-sh/ruff/pull/14717
synced_at: 2026-01-12T15:55:48Z
```

# Update dependency ruff to v0.8.1

---

_@renovate_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.7.4` -> `==0.8.1` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.8.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.8.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.7.4/0.8.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.7.4/0.8.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.8.1`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#081)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.8.0...0.8.1)

##### Preview features

-   Formatter: Avoid invalid syntax for format-spec with quotes for all Python versions ([#&#8203;14625](https://redirect.github.com/astral-sh/ruff/pull/14625))
-   Formatter: Consider quotes inside format-specs when choosing the quotes for an f-string ([#&#8203;14493](https://redirect.github.com/astral-sh/ruff/pull/14493))
-   Formatter: Do not consider f-strings with escaped newlines as multiline ([#&#8203;14624](https://redirect.github.com/astral-sh/ruff/pull/14624))
-   Formatter: Fix f-string formatting in assignment statement ([#&#8203;14454](https://redirect.github.com/astral-sh/ruff/pull/14454))
-   Formatter: Fix unnecessary space around power operator (`**`) in overlong f-string expressions ([#&#8203;14489](https://redirect.github.com/astral-sh/ruff/pull/14489))
-   \[`airflow`] Avoid implicit `schedule` argument to `DAG` and `@dag` (`AIR301`) ([#&#8203;14581](https://redirect.github.com/astral-sh/ruff/pull/14581))
-   \[`flake8-builtins`] Exempt private built-in modules (`A005`) ([#&#8203;14505](https://redirect.github.com/astral-sh/ruff/pull/14505))
-   \[`flake8-pytest-style`] Fix `pytest.mark.parametrize` rules to check calls instead of decorators ([#&#8203;14515](https://redirect.github.com/astral-sh/ruff/pull/14515))
-   \[`flake8-type-checking`] Implement `runtime-cast-value` (`TC006`) ([#&#8203;14511](https://redirect.github.com/astral-sh/ruff/pull/14511))
-   \[`flake8-type-checking`] Implement `unquoted-type-alias` (`TC007`) and `quoted-type-alias` (`TC008`) ([#&#8203;12927](https://redirect.github.com/astral-sh/ruff/pull/12927))
-   \[`flake8-use-pathlib`] Recommend `Path.iterdir()` over `os.listdir()` (`PTH208`) ([#&#8203;14509](https://redirect.github.com/astral-sh/ruff/pull/14509))
-   \[`pylint`] Extend `invalid-envvar-default` to detect `os.environ.get` (`PLW1508`) ([#&#8203;14512](https://redirect.github.com/astral-sh/ruff/pull/14512))
-   \[`pylint`] Implement `len-test` (`PLC1802`) ([#&#8203;14309](https://redirect.github.com/astral-sh/ruff/pull/14309))
-   \[`refurb`] Fix bug where methods defined using lambdas were flagged by `FURB118` ([#&#8203;14639](https://redirect.github.com/astral-sh/ruff/pull/14639))
-   \[`ruff`] Auto-add `r` prefix when string has no backslashes for `unraw-re-pattern` (`RUF039`) ([#&#8203;14536](https://redirect.github.com/astral-sh/ruff/pull/14536))
-   \[`ruff`] Implement `invalid-assert-message-literal-argument` (`RUF040`) ([#&#8203;14488](https://redirect.github.com/astral-sh/ruff/pull/14488))
-   \[`ruff`] Implement `unnecessary-nested-literal` (`RUF041`) ([#&#8203;14323](https://redirect.github.com/astral-sh/ruff/pull/14323))
-   \[`ruff`] Implement `unnecessary-regular-expression` (`RUF055`) ([#&#8203;14659](https://redirect.github.com/astral-sh/ruff/pull/14659))

##### Rule changes

-   Ignore more rules for stub files ([#&#8203;14541](https://redirect.github.com/astral-sh/ruff/pull/14541))
-   \[`pep8-naming`] Eliminate false positives for single-letter names (`N811`, `N814`) ([#&#8203;14584](https://redirect.github.com/astral-sh/ruff/pull/14584))
-   \[`pyflakes`] Avoid false positives in `@no_type_check` contexts (`F821`, `F722`) ([#&#8203;14615](https://redirect.github.com/astral-sh/ruff/pull/14615))
-   \[`ruff`] Detect redirected-noqa in file-level comments (`RUF101`) ([#&#8203;14635](https://redirect.github.com/astral-sh/ruff/pull/14635))
-   \[`ruff`] Mark fixes for `unsorted-dunder-all` and `unsorted-dunder-slots` as unsafe when there are complex comments in the sequence (`RUF022`, `RUF023`) ([#&#8203;14560](https://redirect.github.com/astral-sh/ruff/pull/14560))

##### Bug fixes

-   Avoid fixing code to `None | None` for `redundant-none-literal` (`PYI061`) and `never-union` (`RUF020`) ([#&#8203;14583](https://redirect.github.com/astral-sh/ruff/pull/14583), [#&#8203;14589](https://redirect.github.com/astral-sh/ruff/pull/14589))
-   \[`flake8-bugbear`] Fix `mutable-contextvar-default` to resolve annotated function calls properly (`B039`) ([#&#8203;14532](https://redirect.github.com/astral-sh/ruff/pull/14532))
-   \[`flake8-pyi`, `ruff`] Fix traversal of nested literals and unions (`PYI016`, `PYI051`, `PYI055`, `PYI062`, `RUF041`) ([#&#8203;14641](https://redirect.github.com/astral-sh/ruff/pull/14641))
-   \[`flake8-pyi`] Avoid rewriting invalid type expressions in `unnecessary-type-union` (`PYI055`) ([#&#8203;14660](https://redirect.github.com/astral-sh/ruff/pull/14660))
-   \[`flake8-type-checking`] Avoid syntax errors and type checking problem for quoted annotations autofix (`TC003`, `TC006`) ([#&#8203;14634](https://redirect.github.com/astral-sh/ruff/pull/14634))
-   \[`pylint`] Do not wrap function calls in parentheses in the fix for unnecessary-dunder-call (`PLC2801`) ([#&#8203;14601](https://redirect.github.com/astral-sh/ruff/pull/14601))
-   \[`ruff`] Handle `attrs`'s `auto_attribs` correctly (`RUF009`) ([#&#8203;14520](https://redirect.github.com/astral-sh/ruff/pull/14520))

### [`v0.8.0`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#080)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.7.4...0.8.0)

Check out the [blog post](https://astral.sh/blog/ruff-v0.8.0) for a migration guide and overview of the changes!

##### Breaking changes

See also, the "Remapped rules" section which may result in disabled rules.

-   **Default to Python 3.9**

    Ruff now defaults to Python 3.9 instead of 3.8 if no explicit Python version is configured using [`ruff.target-version`](https://docs.astral.sh/ruff/settings/#target-version) or [`project.requires-python`](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#python-requires) ([#&#8203;13896](https://redirect.github.com/astral-sh/ruff/pull/13896))

-   **Changed location of `pydoclint` diagnostics**

    [`pydoclint`](https://docs.astral.sh/ruff/rules/#pydoclint-doc) diagnostics now point to the first-line of the problematic docstring. Previously, this was not the case.

    If you've opted into these preview rules but have them suppressed using
    [`noqa`](https://docs.astral.sh/ruff/linter/#error-suppression) comments in
    some places, this change may mean that you need to move the `noqa` suppression
    comments. Most users should be unaffected by this change.

-   **Use XDG (i.e. `~/.local/bin`) instead of the Cargo home directory in the standalone installer**

    Previously, Ruff's installer used `$CARGO_HOME` or `~/.cargo/bin` for its target install directory. Now, Ruff will be installed into `$XDG_BIN_HOME`, `$XDG_DATA_HOME/../bin`, or `~/.local/bin` (in that order).

    This change is only relevant to users of the standalone Ruff installer (using the shell or PowerShell script). If you installed Ruff using uv or pip, you should be unaffected.

-   **Changes to the line width calculation**

    Ruff now uses a new version of the [unicode-width](https://redirect.github.com/unicode-rs/unicode-width) Rust crate to calculate the line width. In very rare cases, this may lead to lines containing Unicode characters being reformatted, or being considered too long when they were not before ([`E501`](https://docs.astral.sh/ruff/rules/line-too-long/)).

##### Removed Rules

The following deprecated rules have been removed:

-   [`missing-type-self`](https://docs.astral.sh/ruff/rules/missing-type-self/) (`ANN101`)
-   [`missing-type-cls`](https://docs.astral.sh/ruff/rules/missing-type-cls/) (`ANN102`)
-   [`syntax-error`](https://docs.astral.sh/ruff/rules/syntax-error/) (`E999`)
-   [`pytest-missing-fixture-name-underscore`](https://docs.astral.sh/ruff/rules/pytest-missing-fixture-name-underscore/) (`PT004`)
-   [`pytest-incorrect-fixture-name-underscore`](https://docs.astral.sh/ruff/rules/pytest-incorrect-fixture-name-underscore/) (`PT005`)
-   [`unpacked-list-comprehension`](https://docs.astral.sh/ruff/rules/unpacked-list-comprehension/) (`UP027`)

##### Remapped rules

The following rules have been remapped to new rule codes:

-   [`flake8-type-checking`](https://docs.astral.sh/ruff/rules/#flake8-type-checking-tc): `TCH` to `TC`

##### Stabilization

The following rules have been stabilized and are no longer in preview:

-   [`builtin-import-shadowing`](https://docs.astral.sh/ruff/rules/builtin-import-shadowing/) (`A004`)
-   [`mutable-contextvar-default`](https://docs.astral.sh/ruff/rules/mutable-contextvar-default/) (`B039`)
-   [`fast-api-redundant-response-model`](https://docs.astral.sh/ruff/rules/fast-api-redundant-response-model/) (`FAST001`)
-   [`fast-api-non-annotated-dependency`](https://docs.astral.sh/ruff/rules/fast-api-non-annotated-dependency/) (`FAST002`)
-   [`dict-index-missing-items`](https://docs.astral.sh/ruff/rules/dict-index-missing-items/) (`PLC0206`)
-   [`pep484-style-positional-only-argument`](https://docs.astral.sh/ruff/rules/pep484-style-positional-only-argument/) (`PYI063`)
-   [`redundant-final-literal`](https://docs.astral.sh/ruff/rules/redundant-final-literal/) (`PYI064`)
-   [`bad-version-info-order`](https://docs.astral.sh/ruff/rules/bad-version-info-order/) (`PYI066`)
-   [`parenthesize-chained-operators`](https://docs.astral.sh/ruff/rules/parenthesize-chained-operators/) (`RUF021`)
-   [`unsorted-dunder-all`](https://docs.astral.sh/ruff/rules/unsorted-dunder-all/) (`RUF022`)
-   [`unsorted-dunder-slots`](https://docs.astral.sh/ruff/rules/unsorted-dunder-slots/) (`RUF023`)
-   [`assert-with-print-message`](https://docs.astral.sh/ruff/rules/assert-with-print-message/) (`RUF030`)
-   [`unnecessary-default-type-args`](https://docs.astral.sh/ruff/rules/unnecessary-default-type-args/) (`UP043`)

The following behaviors have been stabilized:

-   [`ambiguous-variable-name`](https://docs.astral.sh/ruff/rules/ambiguous-variable-name/) (`E741`): Violations in stub files are now ignored. Stub authors typically don't control variable names.
-   [`printf-string-formatting`](https://docs.astral.sh/ruff/rules/printf-string-formatting/) (`UP031`): Report all `printf`-like usages even if no autofix is available

The following fixes have been stabilized:

-   [`zip-instead-of-pairwise`](https://docs.astral.sh/ruff/rules/zip-instead-of-pairwise/) (`RUF007`)

##### Preview features

-   \[`flake8-datetimez`] Exempt `min.time()` and `max.time()` (`DTZ901`) ([#&#8203;14394](https://redirect.github.com/astral-sh/ruff/pull/14394))
-   \[`flake8-pie`] Mark fix as unsafe if the following statement is a string literal (`PIE790`) ([#&#8203;14393](https://redirect.github.com/astral-sh/ruff/pull/14393))
-   \[`flake8-pyi`] New rule `redundant-none-literal` (`PYI061`) ([#&#8203;14316](https://redirect.github.com/astral-sh/ruff/pull/14316))
-   \[`flake8-pyi`] Add autofix for `redundant-numeric-union` (`PYI041`) ([#&#8203;14273](https://redirect.github.com/astral-sh/ruff/pull/14273))
-   \[`ruff`] New rule `map-int-version-parsing` (`RUF048`) ([#&#8203;14373](https://redirect.github.com/astral-sh/ruff/pull/14373))
-   \[`ruff`] New rule `redundant-bool-literal` (`RUF038`) ([#&#8203;14319](https://redirect.github.com/astral-sh/ruff/pull/14319))
-   \[`ruff`] New rule `unraw-re-pattern` (`RUF039`) ([#&#8203;14446](https://redirect.github.com/astral-sh/ruff/pull/14446))
-   \[`pycodestyle`] Exempt `pytest.importorskip()` calls (`E402`) ([#&#8203;14474](https://redirect.github.com/astral-sh/ruff/pull/14474))
-   \[`pylint`] Autofix suggests using sets when possible (`PLR1714`) ([#&#8203;14372](https://redirect.github.com/astral-sh/ruff/pull/14372))

##### Rule changes

-   [`invalid-pyproject-toml`](https://docs.astral.sh/ruff/rules/invalid-pyproject-toml/) (`RUF200`): Updated to reflect the provisionally accepted [PEP 639](https://peps.python.org/pep-0639/).
-   \[`flake8-pyi`] Avoid panic in unfixable case (`PYI041`) ([#&#8203;14402](https://redirect.github.com/astral-sh/ruff/pull/14402))
-   \[`flake8-type-checking`] Correctly handle quotes in subscript expression when generating an autofix ([#&#8203;14371](https://redirect.github.com/astral-sh/ruff/pull/14371))
-   \[`pylint`] Suggest correct autofix for `__contains__` (`PLC2801`) ([#&#8203;14424](https://redirect.github.com/astral-sh/ruff/pull/14424))

##### Configuration

-   Ruff now emits a warning instead of an error when a configuration [`ignore`](https://docs.astral.sh/ruff/settings/#lint_ignore)s a rule that has been removed ([#&#8203;14435](https://redirect.github.com/astral-sh/ruff/pull/14435))
-   Ruff now validates that `lint.flake8-import-conventions.aliases` only uses valid module names and aliases ([#&#8203;14477](https://redirect.github.com/astral-sh/ruff/pull/14477))

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

_Label `internal` added by @renovate[bot] on 2024-12-02 01:06_

---

_Merged by @charliermarsh on 2024-12-02 01:13_

---

_Closed by @charliermarsh on 2024-12-02 01:13_

---

_Branch deleted on 2024-12-02 01:13_

---
