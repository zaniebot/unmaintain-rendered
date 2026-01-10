```yaml
number: 15283
title: Update dependency ruff to v0.8.6
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-01-06T00:54:13Z
updated_at: 2025-01-06T01:05:38Z
url: https://github.com/astral-sh/ruff/pull/15283
synced_at: 2026-01-10T20:34:00Z
```

# Update dependency ruff to v0.8.6

---

_Pull request opened by @renovate on 2025-01-06 00:54_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.8.4` -> `==0.8.6` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.8.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.8.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.8.4/0.8.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.8.4/0.8.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.8.6`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#086)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.8.5...0.8.6)

##### Preview features

-   \[`format`]: Preserve multiline implicit concatenated strings in docstring positions ([#&#8203;15126](https://redirect.github.com/astral-sh/ruff/pull/15126))
-   \[`ruff`] Add rule to detect empty literal in deque call (`RUF025`) ([#&#8203;15104](https://redirect.github.com/astral-sh/ruff/pull/15104))
-   \[`ruff`] Avoid reporting when `ndigits` is possibly negative (`RUF057`) ([#&#8203;15234](https://redirect.github.com/astral-sh/ruff/pull/15234))

##### Rule changes

-   \[`flake8-todos`] remove issue code length restriction (`TD003`) ([#&#8203;15175](https://redirect.github.com/astral-sh/ruff/pull/15175))
-   \[`pyflakes`] Ignore errors in `@no_type_check` string annotations (`F722`, `F821`) ([#&#8203;15215](https://redirect.github.com/astral-sh/ruff/pull/15215))

##### CLI

-   Show errors for attempted fixes only when passed `--verbose` ([#&#8203;15237](https://redirect.github.com/astral-sh/ruff/pull/15237))

##### Bug fixes

-   \[`ruff`] Avoid syntax error when removing int over multiple lines (`RUF046`) ([#&#8203;15230](https://redirect.github.com/astral-sh/ruff/pull/15230))
-   \[`pyupgrade`] Revert "Add all PEP-585 names to `UP006` rule" ([#&#8203;15250](https://redirect.github.com/astral-sh/ruff/pull/15250))

### [`v0.8.5`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#085)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.8.4...0.8.5)

##### Preview features

-   \[`airflow`] Extend names moved from core to provider (`AIR303`) ([#&#8203;15145](https://redirect.github.com/astral-sh/ruff/pull/15145), [#&#8203;15159](https://redirect.github.com/astral-sh/ruff/pull/15159), [#&#8203;15196](https://redirect.github.com/astral-sh/ruff/pull/15196), [#&#8203;15216](https://redirect.github.com/astral-sh/ruff/pull/15216))
-   \[`airflow`] Extend rule to check class attributes, methods, arguments (`AIR302`) ([#&#8203;15054](https://redirect.github.com/astral-sh/ruff/pull/15054), [#&#8203;15083](https://redirect.github.com/astral-sh/ruff/pull/15083))
-   \[`fastapi`] Update `FAST002` to check keyword-only arguments ([#&#8203;15119](https://redirect.github.com/astral-sh/ruff/pull/15119))
-   \[`flake8-type-checking`] Disable `TC006` and `TC007` in stub files ([#&#8203;15179](https://redirect.github.com/astral-sh/ruff/pull/15179))
-   \[`pylint`] Detect nested methods correctly (`PLW1641`) ([#&#8203;15032](https://redirect.github.com/astral-sh/ruff/pull/15032))
-   \[`ruff`] Detect more strict-integer expressions (`RUF046`) ([#&#8203;14833](https://redirect.github.com/astral-sh/ruff/pull/14833))
-   \[`ruff`] Implement `falsy-dict-get-fallback` (`RUF056`) ([#&#8203;15160](https://redirect.github.com/astral-sh/ruff/pull/15160))
-   \[`ruff`] Implement `unnecessary-round` (`RUF057`) ([#&#8203;14828](https://redirect.github.com/astral-sh/ruff/pull/14828))

##### Rule changes

-   Visit PEP 764 inline `TypedDict` keys as non-type-expressions ([#&#8203;15073](https://redirect.github.com/astral-sh/ruff/pull/15073))
-   \[`flake8-comprehensions`] Skip `C416` if comprehension contains unpacking ([#&#8203;14909](https://redirect.github.com/astral-sh/ruff/pull/14909))
-   \[`flake8-pie`] Allow `cast(SomeType, ...)` (`PIE796`) ([#&#8203;15141](https://redirect.github.com/astral-sh/ruff/pull/15141))
-   \[`flake8-simplify`] More precise inference for dictionaries (`SIM300`) ([#&#8203;15164](https://redirect.github.com/astral-sh/ruff/pull/15164))
-   \[`flake8-use-pathlib`] Catch redundant joins in `PTH201` and avoid syntax errors ([#&#8203;15177](https://redirect.github.com/astral-sh/ruff/pull/15177))
-   \[`pycodestyle`] Preserve original value format (`E731`) ([#&#8203;15097](https://redirect.github.com/astral-sh/ruff/pull/15097))
-   \[`pydocstyle`] Split on first whitespace character (`D403`) ([#&#8203;15082](https://redirect.github.com/astral-sh/ruff/pull/15082))
-   \[`pyupgrade`] Add all PEP-585 names to `UP006` rule ([#&#8203;5454](https://redirect.github.com/astral-sh/ruff/pull/5454))

##### Configuration

-   \[`flake8-type-checking`] Improve flexibility of `runtime-evaluated-decorators` ([#&#8203;15204](https://redirect.github.com/astral-sh/ruff/pull/15204))
-   \[`pydocstyle`] Add setting to ignore missing documentation for `*args` and `**kwargs` parameters (`D417`) ([#&#8203;15210](https://redirect.github.com/astral-sh/ruff/pull/15210))
-   \[`ruff`] Add an allowlist for `unsafe-markup-use` (`RUF035`) ([#&#8203;15076](https://redirect.github.com/astral-sh/ruff/pull/15076))

##### Bug fixes

-   Fix type subscript on older python versions ([#&#8203;15090](https://redirect.github.com/astral-sh/ruff/pull/15090))
-   Use `TypeChecker` for detecting `fastapi` routes ([#&#8203;15093](https://redirect.github.com/astral-sh/ruff/pull/15093))
-   \[`pycodestyle`] Avoid false positives and negatives related to type parameter default syntax (`E225`, `E251`) ([#&#8203;15214](https://redirect.github.com/astral-sh/ruff/pull/15214))

##### Documentation

-   Fix incorrect doc in `shebang-not-executable` (`EXE001`) and add git+windows solution to executable bit ([#&#8203;15208](https://redirect.github.com/astral-sh/ruff/pull/15208))
-   Rename rules currently not conforming to naming convention ([#&#8203;15102](https://redirect.github.com/astral-sh/ruff/pull/15102))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS44NS4wIiwidXBkYXRlZEluVmVyIjoiMzkuODUuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-01-06 00:54_

---

_Merged by @charliermarsh on 2025-01-06 01:05_

---

_Closed by @charliermarsh on 2025-01-06 01:05_

---

_Branch deleted on 2025-01-06 01:05_

---
