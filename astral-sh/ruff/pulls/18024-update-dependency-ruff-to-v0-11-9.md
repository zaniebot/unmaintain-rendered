```yaml
number: 18024
title: Update dependency ruff to v0.11.9
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-05-12T02:00:33Z
updated_at: 2025-05-12T02:03:57Z
url: https://github.com/astral-sh/ruff/pull/18024
synced_at: 2026-01-10T18:57:03Z
```

# Update dependency ruff to v0.11.9

---

_Pull request opened by @renovate on 2025-05-12 02:00_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.11.8` -> `==0.11.9` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.11.9?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.11.9?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.11.8/0.11.9?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.11.8/0.11.9?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.11.9`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0119)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.11.8...0.11.9)

##### Preview features

-   Default to latest supported Python version for version-related syntax errors ([#&#8203;17529](https://redirect.github.com/astral-sh/ruff/pull/17529))
-   Implement deferred annotations for Python 3.14 ([#&#8203;17658](https://redirect.github.com/astral-sh/ruff/pull/17658))
-   \[`airflow`] Fix `SQLTableCheckOperator` typo (`AIR302`) ([#&#8203;17946](https://redirect.github.com/astral-sh/ruff/pull/17946))
-   \[`airflow`] Remove `airflow.utils.dag_parsing_context.get_parsing_context` (`AIR301`) ([#&#8203;17852](https://redirect.github.com/astral-sh/ruff/pull/17852))
-   \[`airflow`] Skip attribute check in try catch block (`AIR301`) ([#&#8203;17790](https://redirect.github.com/astral-sh/ruff/pull/17790))
-   \[`flake8-bandit`] Mark tuples of string literals as trusted input in `S603` ([#&#8203;17801](https://redirect.github.com/astral-sh/ruff/pull/17801))
-   \[`isort`] Check full module path against project root(s) when categorizing first-party imports ([#&#8203;16565](https://redirect.github.com/astral-sh/ruff/pull/16565))
-   \[`ruff`] Add new rule `in-empty-collection` (`RUF060`) ([#&#8203;16480](https://redirect.github.com/astral-sh/ruff/pull/16480))

##### Bug fixes

-   Fix missing `combine` call for `lint.typing-extensions` setting ([#&#8203;17823](https://redirect.github.com/astral-sh/ruff/pull/17823))
-   \[`flake8-async`] Fix module name in `ASYNC110`, `ASYNC115`, and `ASYNC116` fixes ([#&#8203;17774](https://redirect.github.com/astral-sh/ruff/pull/17774))
-   \[`pyupgrade`] Add spaces between tokens as necessary to avoid syntax errors in `UP018` autofix ([#&#8203;17648](https://redirect.github.com/astral-sh/ruff/pull/17648))
-   \[`refurb`] Fix false positive for float and complex numbers in `FURB116` ([#&#8203;17661](https://redirect.github.com/astral-sh/ruff/pull/17661))
-   \[parser] Flag single unparenthesized generator expr with trailing comma in arguments. ([#&#8203;17893](https://redirect.github.com/astral-sh/ruff/pull/17893))

##### Documentation

-   Add instructions on how to upgrade to a newer Rust version ([#&#8203;17928](https://redirect.github.com/astral-sh/ruff/pull/17928))
-   Update code of conduct email address ([#&#8203;17875](https://redirect.github.com/astral-sh/ruff/pull/17875))
-   Add fix safety sections to `PLC2801`, `PLR1722`, and `RUF013` ([#&#8203;17825](https://redirect.github.com/astral-sh/ruff/pull/17825), [#&#8203;17826](https://redirect.github.com/astral-sh/ruff/pull/17826), [#&#8203;17759](https://redirect.github.com/astral-sh/ruff/pull/17759))
-   Add link to `check-typed-exception` from `S110` and `S112` ([#&#8203;17786](https://redirect.github.com/astral-sh/ruff/pull/17786))

##### Other changes

-   Allow passing a virtual environment to `ruff analyze graph` ([#&#8203;17743](https://redirect.github.com/astral-sh/ruff/pull/17743))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MC43LjEiLCJ1cGRhdGVkSW5WZXIiOiI0MC43LjEiLCJ0YXJnZXRCcmFuY2giOiJtYWluIiwibGFiZWxzIjpbImludGVybmFsIl19-->


---

_Label `internal` added by @renovate[bot] on 2025-05-12 02:00_

---

_Merged by @AlexWaygood on 2025-05-12 02:03_

---

_Closed by @AlexWaygood on 2025-05-12 02:03_

---

_Branch deleted on 2025-05-12 02:03_

---
