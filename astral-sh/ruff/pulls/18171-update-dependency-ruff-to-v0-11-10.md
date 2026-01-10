```yaml
number: 18171
title: Update dependency ruff to v0.11.10
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-05-19T05:51:22Z
updated_at: 2025-05-19T06:29:05Z
url: https://github.com/astral-sh/ruff/pull/18171
synced_at: 2026-01-10T18:51:01Z
```

# Update dependency ruff to v0.11.10

---

_Pull request opened by @renovate on 2025-05-19 05:51_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.11.9` -> `==0.11.10` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.11.10?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.11.10?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.11.9/0.11.10?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.11.9/0.11.10?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.11.10`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#01110)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.11.9...0.11.10)

##### Preview features

-   \[`ruff`] Implement a recursive check for `RUF060` ([#&#8203;17976](https://redirect.github.com/astral-sh/ruff/pull/17976))
-   \[`airflow`] Enable autofixes for `AIR301` and `AIR311` ([#&#8203;17941](https://redirect.github.com/astral-sh/ruff/pull/17941))
-   \[`airflow`] Apply try catch guard to all `AIR3` rules ([#&#8203;17887](https://redirect.github.com/astral-sh/ruff/pull/17887))
-   \[`airflow`] Extend `AIR311` rules ([#&#8203;17913](https://redirect.github.com/astral-sh/ruff/pull/17913))

##### Bug fixes

-   \[`flake8-bugbear`] Ignore `B028` if `skip_file_prefixes` is present ([#&#8203;18047](https://redirect.github.com/astral-sh/ruff/pull/18047))
-   \[`flake8-pie`] Mark autofix for `PIE804` as unsafe if the dictionary contains comments ([#&#8203;18046](https://redirect.github.com/astral-sh/ruff/pull/18046))
-   \[`flake8-simplify`] Correct behavior for `str.split`/`rsplit` with `maxsplit=0` (`SIM905`) ([#&#8203;18075](https://redirect.github.com/astral-sh/ruff/pull/18075))
-   \[`flake8-simplify`] Fix `SIM905` autofix for `rsplit` creating a reversed list literal ([#&#8203;18045](https://redirect.github.com/astral-sh/ruff/pull/18045))
-   \[`flake8-use-pathlib`] Suppress diagnostics for all `os.*` functions that have the `dir_fd` parameter (`PTH`) ([#&#8203;17968](https://redirect.github.com/astral-sh/ruff/pull/17968))
-   \[`refurb`] Mark autofix as safe only for number literals (`FURB116`) ([#&#8203;17692](https://redirect.github.com/astral-sh/ruff/pull/17692))

##### Rule changes

-   \[`flake8-bandit`] Skip `S608` for expressionless f-strings ([#&#8203;17999](https://redirect.github.com/astral-sh/ruff/pull/17999))
-   \[`flake8-pytest-style`] Don't recommend `usefixtures` for `parametrize` values (`PT019`) ([#&#8203;17650](https://redirect.github.com/astral-sh/ruff/pull/17650))
-   \[`pyupgrade`] Add `resource.error` as deprecated alias of `OSError` (`UP024`) ([#&#8203;17933](https://redirect.github.com/astral-sh/ruff/pull/17933))

##### CLI

-   Disable jemalloc on Android ([#&#8203;18033](https://redirect.github.com/astral-sh/ruff/pull/18033))

##### Documentation

-   Update Neovim setup docs ([#&#8203;18108](https://redirect.github.com/astral-sh/ruff/pull/18108))
-   \[`flake8-simplify`] Add fix safety section (`SIM103`) ([#&#8203;18086](https://redirect.github.com/astral-sh/ruff/pull/18086))
-   \[`flake8-simplify`] Add fix safety section (`SIM112`) ([#&#8203;18099](https://redirect.github.com/astral-sh/ruff/pull/18099))
-   \[`pylint`] Add fix safety section (`PLC0414`) ([#&#8203;17802](https://redirect.github.com/astral-sh/ruff/pull/17802))
-   \[`pylint`] Add fix safety section (`PLE4703`) ([#&#8203;17824](https://redirect.github.com/astral-sh/ruff/pull/17824))
-   \[`pylint`] Add fix safety section (`PLW1514`) ([#&#8203;17932](https://redirect.github.com/astral-sh/ruff/pull/17932))
-   \[`pylint`] Add fix safety section (`PLW3301`) ([#&#8203;17878](https://redirect.github.com/astral-sh/ruff/pull/17878))
-   \[`ruff`] Add fix safety section (`RUF007`) ([#&#8203;17755](https://redirect.github.com/astral-sh/ruff/pull/17755))
-   \[`ruff`] Add fix safety section (`RUF033`) ([#&#8203;17760](https://redirect.github.com/astral-sh/ruff/pull/17760))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MC4xMS4xOCIsInVwZGF0ZWRJblZlciI6IjQwLjExLjE4IiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-05-19 05:51_

---

_Merged by @MichaReiser on 2025-05-19 06:29_

---

_Closed by @MichaReiser on 2025-05-19 06:29_

---

_Branch deleted on 2025-05-19 06:29_

---
