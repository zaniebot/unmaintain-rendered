```yaml
number: 16336
title: Update dependency ruff to v0.9.7
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-02-24T03:04:46Z
updated_at: 2025-02-24T06:31:57Z
url: https://github.com/astral-sh/ruff/pull/16336
synced_at: 2026-01-10T19:49:01Z
```

# Update dependency ruff to v0.9.7

---

_Pull request opened by @renovate on 2025-02-24 03:04_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.9.6` -> `==0.9.7` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.9.7?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.9.7?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.9.6/0.9.7?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.9.6/0.9.7?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.9.7`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#097)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.9.6...0.9.7)

##### Preview features

-   Consider `__new__` methods as special function type for enforcing class method or static method rules ([#&#8203;13305](https://redirect.github.com/astral-sh/ruff/pull/13305))
-   \[`airflow`] Improve the internal logic to differentiate deprecated symbols (`AIR303`) ([#&#8203;16013](https://redirect.github.com/astral-sh/ruff/pull/16013))
-   \[`refurb`] Manual timezone monkeypatching (`FURB162`) ([#&#8203;16113](https://redirect.github.com/astral-sh/ruff/pull/16113))
-   \[`ruff`] Implicit class variable in dataclass (`RUF045`) ([#&#8203;14349](https://redirect.github.com/astral-sh/ruff/pull/14349))
-   \[`ruff`] Skip singleton starred expressions for `incorrectly-parenthesized-tuple-in-subscript` (`RUF031`) ([#&#8203;16083](https://redirect.github.com/astral-sh/ruff/pull/16083))
-   \[`refurb`] Check for subclasses includes subscript expressions (`FURB189`) ([#&#8203;16155](https://redirect.github.com/astral-sh/ruff/pull/16155))

##### Rule changes

-   \[`flake8-comprehensions`]: Handle trailing comma in `C403` fix ([#&#8203;16110](https://redirect.github.com/astral-sh/ruff/pull/16110))
-   \[`flake8-debugger`] Also flag `sys.breakpointhook` and `sys.__breakpointhook__` (`T100`) ([#&#8203;16191](https://redirect.github.com/astral-sh/ruff/pull/16191))
-   \[`pydocstyle`] Handle arguments with the same names as sections (`D417`) ([#&#8203;16011](https://redirect.github.com/astral-sh/ruff/pull/16011))
-   \[`pylint`] Correct ordering of arguments in fix for `if-stmt-min-max` (`PLR1730`) ([#&#8203;16080](https://redirect.github.com/astral-sh/ruff/pull/16080))
-   \[`pylint`] Do not offer fix for raw strings (`PLE251`) ([#&#8203;16132](https://redirect.github.com/astral-sh/ruff/pull/16132))
-   \[`pyupgrade`] Do not upgrade functional `TypedDicts` with private field names to the class-based syntax (`UP013`) ([#&#8203;16219](https://redirect.github.com/astral-sh/ruff/pull/16219))
-   \[`pyupgrade`] Handle micro version numbers correctly (`UP036`) ([#&#8203;16091](https://redirect.github.com/astral-sh/ruff/pull/16091))
-   \[`pyupgrade`] Unwrap unary expressions correctly (`UP018`) ([#&#8203;15919](https://redirect.github.com/astral-sh/ruff/pull/15919))
-   \[`ruff`] Skip `RUF001` diagnostics when visiting string type definitions ([#&#8203;16122](https://redirect.github.com/astral-sh/ruff/pull/16122))
-   \[`flake8-pyi`] Avoid flagging `custom-typevar-for-self` on metaclass methods (`PYI019`) ([#&#8203;16141](https://redirect.github.com/astral-sh/ruff/pull/16141))
-   \[`pycodestyle`] Exempt `site.addsitedir(...)` calls (`E402`) ([#&#8203;16251](https://redirect.github.com/astral-sh/ruff/pull/16251))

##### Formatter

-   Fix unstable formatting of trailing end-of-line comments of parenthesized attribute values ([#&#8203;16187](https://redirect.github.com/astral-sh/ruff/pull/16187))

##### Server

-   Fix handling of requests received after shutdown message ([#&#8203;16262](https://redirect.github.com/astral-sh/ruff/pull/16262))
-   Ignore `source.organizeImports.ruff` and `source.fixAll.ruff` code actions for a notebook cell ([#&#8203;16154](https://redirect.github.com/astral-sh/ruff/pull/16154))
-   Include document specific debug info for `ruff.printDebugInformation` ([#&#8203;16215](https://redirect.github.com/astral-sh/ruff/pull/16215))
-   Update server to return the debug info as string with `ruff.printDebugInformation` ([#&#8203;16214](https://redirect.github.com/astral-sh/ruff/pull/16214))

##### CLI

-   Warn on invalid `noqa` even when there are no diagnostics ([#&#8203;16178](https://redirect.github.com/astral-sh/ruff/pull/16178))
-   Better error messages while loading configuration `extend`s ([#&#8203;15658](https://redirect.github.com/astral-sh/ruff/pull/15658))

##### Bug fixes

-   \[`refurb`] Correctly handle lengths of literal strings in `slice-to-remove-prefix-or-suffix` (`FURB188`) ([#&#8203;16237](https://redirect.github.com/astral-sh/ruff/pull/16237))

##### Documentation

-   Add FAQ entry for `source.*` code actions in Notebook ([#&#8203;16212](https://redirect.github.com/astral-sh/ruff/pull/16212))
-   Add `SECURITY.md` ([#&#8203;16224](https://redirect.github.com/astral-sh/ruff/pull/16224))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xNzYuMiIsInVwZGF0ZWRJblZlciI6IjM5LjE3Ni4yIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-02-24 03:04_

---

_Merged by @dhruvmanila on 2025-02-24 06:31_

---

_Closed by @dhruvmanila on 2025-02-24 06:31_

---

_Branch deleted on 2025-02-24 06:31_

---
