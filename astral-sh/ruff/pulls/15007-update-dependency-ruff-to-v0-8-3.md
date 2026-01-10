```yaml
number: 15007
title: Update dependency ruff to v0.8.3
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2024-12-16T01:15:15Z
updated_at: 2024-12-16T01:25:29Z
url: https://github.com/astral-sh/ruff/pull/15007
synced_at: 2026-01-10T20:42:27Z
```

# Update dependency ruff to v0.8.3

---

_Pull request opened by @renovate on 2024-12-16 01:15_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.8.2` -> `==0.8.3` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.8.3?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.8.3?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.8.2/0.8.3?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.8.2/0.8.3?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.8.3`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#083)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.8.2...0.8.3)

##### Preview features

-   Fix fstring formatting removing overlong implicit concatenated string in expression part ([#&#8203;14811](https://redirect.github.com/astral-sh/ruff/pull/14811))
-   \[`airflow`] Add fix to remove deprecated keyword arguments (`AIR302`) ([#&#8203;14887](https://redirect.github.com/astral-sh/ruff/pull/14887))
-   \[`airflow`]: Extend rule to include deprecated names for Airflow 3.0 (`AIR302`) ([#&#8203;14765](https://redirect.github.com/astral-sh/ruff/pull/14765) and [#&#8203;14804](https://redirect.github.com/astral-sh/ruff/pull/14804))
-   \[`flake8-bugbear`] Improve error messages for `except*` (`B025`, `B029`, `B030`, `B904`) ([#&#8203;14815](https://redirect.github.com/astral-sh/ruff/pull/14815))
-   \[`flake8-bugbear`] `itertools.batched()` without explicit `strict` (`B911`) ([#&#8203;14408](https://redirect.github.com/astral-sh/ruff/pull/14408))
-   \[`flake8-use-pathlib`] Dotless suffix passed to `Path.with_suffix()` (`PTH210`) ([#&#8203;14779](https://redirect.github.com/astral-sh/ruff/pull/14779))
-   \[`pylint`] Include parentheses and multiple comparators in check for `boolean-chained-comparison` (`PLR1716`) ([#&#8203;14781](https://redirect.github.com/astral-sh/ruff/pull/14781))
-   \[`ruff`] Do not simplify `round()` calls (`RUF046`) ([#&#8203;14832](https://redirect.github.com/astral-sh/ruff/pull/14832))
-   \[`ruff`] Don't emit `used-dummy-variable` on function parameters (`RUF052`) ([#&#8203;14818](https://redirect.github.com/astral-sh/ruff/pull/14818))
-   \[`ruff`] Implement `if-key-in-dict-del` (`RUF051`) ([#&#8203;14553](https://redirect.github.com/astral-sh/ruff/pull/14553))
-   \[`ruff`] Mark autofix for `RUF052` as always unsafe ([#&#8203;14824](https://redirect.github.com/astral-sh/ruff/pull/14824))
-   \[`ruff`] Teach autofix for `used-dummy-variable` about TypeVars etc. (`RUF052`) ([#&#8203;14819](https://redirect.github.com/astral-sh/ruff/pull/14819))

##### Rule changes

-   \[`flake8-bugbear`] Offer unsafe autofix for `no-explicit-stacklevel` (`B028`) ([#&#8203;14829](https://redirect.github.com/astral-sh/ruff/pull/14829))
-   \[`flake8-pyi`] Skip all type definitions in `string-or-bytes-too-long` (`PYI053`) ([#&#8203;14797](https://redirect.github.com/astral-sh/ruff/pull/14797))
-   \[`pyupgrade`] Do not report when a UTF-8 comment is followed by a non-UTF-8 one (`UP009`) ([#&#8203;14728](https://redirect.github.com/astral-sh/ruff/pull/14728))
-   \[`pyupgrade`] Mark fixes for `convert-typed-dict-functional-to-class` and `convert-named-tuple-functional-to-class` as unsafe if they will remove comments (`UP013`, `UP014`) ([#&#8203;14842](https://redirect.github.com/astral-sh/ruff/pull/14842))

##### Bug fixes

-   Raise syntax error for mixing `except` and `except*` ([#&#8203;14895](https://redirect.github.com/astral-sh/ruff/pull/14895))
-   \[`flake8-bugbear`] Fix `B028` to allow `stacklevel` to be explicitly assigned as a positional argument ([#&#8203;14868](https://redirect.github.com/astral-sh/ruff/pull/14868))
-   \[`flake8-bugbear`] Skip `B028` if `warnings.warn` is called with `*args` or `**kwargs` ([#&#8203;14870](https://redirect.github.com/astral-sh/ruff/pull/14870))
-   \[`flake8-comprehensions`] Skip iterables with named expressions in `unnecessary-map` (`C417`) ([#&#8203;14827](https://redirect.github.com/astral-sh/ruff/pull/14827))
-   \[`flake8-pyi`] Also remove `self` and `cls`'s annotation (`PYI034`) ([#&#8203;14801](https://redirect.github.com/astral-sh/ruff/pull/14801))
-   \[`flake8-pytest-style`] Fix `pytest-parametrize-names-wrong-type` (`PT006`) to edit both `argnames` and `argvalues` if both of them are single-element tuples/lists ([#&#8203;14699](https://redirect.github.com/astral-sh/ruff/pull/14699))
-   \[`perflint`] Improve autofix for `PERF401` ([#&#8203;14369](https://redirect.github.com/astral-sh/ruff/pull/14369))
-   \[`pylint`] Fix `PLW1508` false positive for default string created via a mult operation ([#&#8203;14841](https://redirect.github.com/astral-sh/ruff/pull/14841))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS41OC4xIiwidXBkYXRlZEluVmVyIjoiMzkuNTguMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2024-12-16 01:15_

---

_Merged by @charliermarsh on 2024-12-16 01:25_

---

_Closed by @charliermarsh on 2024-12-16 01:25_

---

_Branch deleted on 2024-12-16 01:25_

---
