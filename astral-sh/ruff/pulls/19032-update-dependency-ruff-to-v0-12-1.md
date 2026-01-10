```yaml
number: 19032
title: Update dependency ruff to v0.12.1
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-06-30T01:00:29Z
updated_at: 2025-06-30T07:42:55Z
url: https://github.com/astral-sh/ruff/pull/19032
synced_at: 2026-01-10T18:39:09Z
```

# Update dependency ruff to v0.12.1

---

_Pull request opened by @renovate on 2025-06-30 01:00_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.12.0` -> `==0.12.1` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.12.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.12.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.12.0/0.12.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.12.0/0.12.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.12.1`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0121)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.12.0...0.12.1)

##### Preview features

- \[`flake8-errmsg`] Extend `EM101` to support byte strings ([#&#8203;18867](https://redirect.github.com/astral-sh/ruff/pull/18867))
- \[`flake8-use-pathlib`] Add autofix for `PTH202` ([#&#8203;18763](https://redirect.github.com/astral-sh/ruff/pull/18763))
- \[`pygrep-hooks`] Add `AsyncMock` methods to `invalid-mock-access` (`PGH005`) ([#&#8203;18547](https://redirect.github.com/astral-sh/ruff/pull/18547))
- \[`pylint`] Ignore `__init__.py` files in (`PLC0414`) ([#&#8203;18400](https://redirect.github.com/astral-sh/ruff/pull/18400))
- \[`ruff`] Trigger `RUF037` for empty string and byte strings ([#&#8203;18862](https://redirect.github.com/astral-sh/ruff/pull/18862))
- \[formatter] Fix missing blank lines before decorated classes in `.pyi` files ([#&#8203;18888](https://redirect.github.com/astral-sh/ruff/pull/18888))

##### Bug fixes

- Avoid generating diagnostics with per-file ignores ([#&#8203;18801](https://redirect.github.com/astral-sh/ruff/pull/18801))
- Handle parenthesized arguments in `remove_argument` ([#&#8203;18805](https://redirect.github.com/astral-sh/ruff/pull/18805))
- \[`flake8-logging`] Avoid false positive for `exc_info=True` outside `logger.exception` (`LOG014`) ([#&#8203;18737](https://redirect.github.com/astral-sh/ruff/pull/18737))
- \[`flake8-pytest-style`] Enforce `pytest` import for decorators ([#&#8203;18779](https://redirect.github.com/astral-sh/ruff/pull/18779))
- \[`flake8-pytest-style`] Mark autofix for `PT001` and `PT023` as unsafe if there's comments in the decorator ([#&#8203;18792](https://redirect.github.com/astral-sh/ruff/pull/18792))
- \[`flake8-pytest-style`] `PT001`/`PT023` fix makes syntax error on parenthesized decorator ([#&#8203;18782](https://redirect.github.com/astral-sh/ruff/pull/18782))
- \[`flake8-raise`] Make fix unsafe if it deletes comments (`RSE102`) ([#&#8203;18788](https://redirect.github.com/astral-sh/ruff/pull/18788))
- \[`flake8-simplify`] Fix `SIM911` autofix creating a syntax error ([#&#8203;18793](https://redirect.github.com/astral-sh/ruff/pull/18793))
- \[`flake8-simplify`] Fix false negatives for shadowed bindings (`SIM910`, `SIM911`) ([#&#8203;18794](https://redirect.github.com/astral-sh/ruff/pull/18794))
- \[`flake8-simplify`] Preserve original behavior for `except ()` and bare `except` (`SIM105`) ([#&#8203;18213](https://redirect.github.com/astral-sh/ruff/pull/18213))
- \[`flake8-pyi`] Fix `PYI041`'s fix causing `TypeError` with `None | None | ...` ([#&#8203;18637](https://redirect.github.com/astral-sh/ruff/pull/18637))
- \[`perflint`] Fix `PERF101` autofix creating a syntax error and mark autofix as unsafe if there are comments in the `list` call expr ([#&#8203;18803](https://redirect.github.com/astral-sh/ruff/pull/18803))
- \[`perflint`] Fix false negative in `PERF401` ([#&#8203;18866](https://redirect.github.com/astral-sh/ruff/pull/18866))
- \[`pylint`] Avoid flattening nested `min`/`max` when outer call has single argument (`PLW3301`) ([#&#8203;16885](https://redirect.github.com/astral-sh/ruff/pull/16885))
- \[`pylint`] Fix `PLC2801` autofix creating a syntax error ([#&#8203;18857](https://redirect.github.com/astral-sh/ruff/pull/18857))
- \[`pylint`] Mark `PLE0241` autofix as unsafe if there's comments in the base classes ([#&#8203;18832](https://redirect.github.com/astral-sh/ruff/pull/18832))
- \[`pylint`] Suppress `PLE2510`/`PLE2512`/`PLE2513`/`PLE2514`/`PLE2515` autofix if the text contains an odd number of backslashes ([#&#8203;18856](https://redirect.github.com/astral-sh/ruff/pull/18856))
- \[`refurb`] Detect more exotic float literals in `FURB164` ([#&#8203;18925](https://redirect.github.com/astral-sh/ruff/pull/18925))
- \[`refurb`] Fix `FURB163` autofix creating a syntax error for `yield` expressions ([#&#8203;18756](https://redirect.github.com/astral-sh/ruff/pull/18756))
- \[`refurb`] Mark `FURB129` autofix as unsafe if there's comments in the `readlines` call ([#&#8203;18858](https://redirect.github.com/astral-sh/ruff/pull/18858))
- \[`ruff`] Fix false positives and negatives in `RUF010` ([#&#8203;18690](https://redirect.github.com/astral-sh/ruff/pull/18690))
- Fix casing of `analyze.direction` variant names ([#&#8203;18892](https://redirect.github.com/astral-sh/ruff/pull/18892))

##### Rule changes

- Fix f-string interpolation escaping in generated fixes ([#&#8203;18882](https://redirect.github.com/astral-sh/ruff/pull/18882))
- \[`flake8-return`] Mark `RET501` fix unsafe if comments are inside ([#&#8203;18780](https://redirect.github.com/astral-sh/ruff/pull/18780))
- \[`flake8-async`] Fix detection for large integer sleep durations in `ASYNC116` rule ([#&#8203;18767](https://redirect.github.com/astral-sh/ruff/pull/18767))
- \[`flake8-async`] Mark autofix for `ASYNC115` as unsafe if the call expression contains comments ([#&#8203;18753](https://redirect.github.com/astral-sh/ruff/pull/18753))
- \[`flake8-bugbear`] Mark autofix for `B004` as unsafe if the `hasattr` call expr contains comments ([#&#8203;18755](https://redirect.github.com/astral-sh/ruff/pull/18755))
- \[`flake8-comprehension`] Mark autofix for `C420` as unsafe if there's comments inside the dict comprehension ([#&#8203;18768](https://redirect.github.com/astral-sh/ruff/pull/18768))
- \[`flake8-comprehensions`] Handle template strings for comprehension fixes ([#&#8203;18710](https://redirect.github.com/astral-sh/ruff/pull/18710))
- \[`flake8-future-annotations`] Add autofix (`FA100`) ([#&#8203;18903](https://redirect.github.com/astral-sh/ruff/pull/18903))
- \[`pyflakes`] Mark `F504`/`F522`/`F523` autofix as unsafe if there's a call with side effect ([#&#8203;18839](https://redirect.github.com/astral-sh/ruff/pull/18839))
- \[`pylint`] Allow fix with comments and document performance implications (`PLW3301`) ([#&#8203;18936](https://redirect.github.com/astral-sh/ruff/pull/18936))
- \[`pylint`] Detect more exotic `NaN` literals in `PLW0177` ([#&#8203;18630](https://redirect.github.com/astral-sh/ruff/pull/18630))
- \[`pylint`] Fix `PLC1802` autofix creating a syntax error and mark autofix as unsafe if there's comments in the `len` call ([#&#8203;18836](https://redirect.github.com/astral-sh/ruff/pull/18836))
- \[`pyupgrade`] Extend version detection to include `sys.version_info.major` (`UP036`) ([#&#8203;18633](https://redirect.github.com/astral-sh/ruff/pull/18633))
- \[`ruff`] Add lint rule `RUF064` for calling `chmod` with non-octal integers ([#&#8203;18541](https://redirect.github.com/astral-sh/ruff/pull/18541))
- \[`ruff`] Added `cls.__dict__.get('__annotations__')` check (`RUF063`) ([#&#8203;18233](https://redirect.github.com/astral-sh/ruff/pull/18233))
- \[`ruff`] Frozen `dataclass` default should be valid (`RUF009`) ([#&#8203;18735](https://redirect.github.com/astral-sh/ruff/pull/18735))

##### Server

- Consider virtual path for various server actions ([#&#8203;18910](https://redirect.github.com/astral-sh/ruff/pull/18910))

##### Documentation

- Add fix safety sections ([#&#8203;18940](https://redirect.github.com/astral-sh/ruff/pull/18940),[#&#8203;18841](https://redirect.github.com/astral-sh/ruff/pull/18841),[#&#8203;18802](https://redirect.github.com/astral-sh/ruff/pull/18802),[#&#8203;18837](https://redirect.github.com/astral-sh/ruff/pull/18837),[#&#8203;18800](https://redirect.github.com/astral-sh/ruff/pull/18800),[#&#8203;18415](https://redirect.github.com/astral-sh/ruff/pull/18415),[#&#8203;18853](https://redirect.github.com/astral-sh/ruff/pull/18853),[#&#8203;18842](https://redirect.github.com/astral-sh/ruff/pull/18842))
- Use updated pre-commit id ([#&#8203;18718](https://redirect.github.com/astral-sh/ruff/pull/18718))
- \[`perflint`] Small docs improvement to `PERF401` ([#&#8203;18786](https://redirect.github.com/astral-sh/ruff/pull/18786))
- \[`pyupgrade`]: Use `super()`, not `__super__` in error messages (`UP008`) ([#&#8203;18743](https://redirect.github.com/astral-sh/ruff/pull/18743))
- \[`flake8-pie`] Small docs fix to `PIE794` ([#&#8203;18829](https://redirect.github.com/astral-sh/ruff/pull/18829))
- \[`flake8-pyi`] Correct `collections-named-tuple` example to use PascalCase assignment ([#&#8203;16884](https://redirect.github.com/astral-sh/ruff/pull/16884))
- \[`flake8-pie`] Add note on type checking benefits to `unnecessary-dict-kwargs` (`PIE804`) ([#&#8203;18666](https://redirect.github.com/astral-sh/ruff/pull/18666))
- \[`pycodestyle`] Clarify PEP 8 relationship to `whitespace-around-operator` rules ([#&#8203;18870](https://redirect.github.com/astral-sh/ruff/pull/18870))

##### Other changes

- Disallow newlines in format specifiers of single quoted f- or t-strings ([#&#8203;18708](https://redirect.github.com/astral-sh/ruff/pull/18708))
- \[`flake8-logging`] Add fix safety section to `LOG002` ([#&#8203;18840](https://redirect.github.com/astral-sh/ruff/pull/18840))
- \[`pyupgrade`] Add fix safety section to `UP010` ([#&#8203;18838](https://redirect.github.com/astral-sh/ruff/pull/18838))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MC42Mi4xIiwidXBkYXRlZEluVmVyIjoiNDAuNjIuMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-06-30 01:00_

---

_Merged by @dhruvmanila on 2025-06-30 07:42_

---

_Closed by @dhruvmanila on 2025-06-30 07:42_

---

_Branch deleted on 2025-06-30 07:42_

---
