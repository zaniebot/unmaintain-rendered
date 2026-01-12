```yaml
number: 16197
title: Update dependency ruff to v0.9.6
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-02-17T03:42:59Z
updated_at: 2025-02-17T07:21:32Z
url: https://github.com/astral-sh/ruff/pull/16197
synced_at: 2026-01-12T15:55:54Z
```

# Update dependency ruff to v0.9.6

---

_@renovate_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.9.5` -> `==0.9.6` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.9.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.9.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.9.5/0.9.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.9.5/0.9.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.9.6`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#096)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.9.5...0.9.6)

##### Preview features

-   \[`airflow`] Add `external_task.{ExternalTaskMarker, ExternalTaskSensor}` for `AIR302` ([#&#8203;16014](https://redirect.github.com/astral-sh/ruff/pull/16014))
-   \[`flake8-builtins`] Make strict module name comparison optional (`A005`) ([#&#8203;15951](https://redirect.github.com/astral-sh/ruff/pull/15951))
-   \[`flake8-pyi`] Extend fix to Python <= 3.9 for `redundant-none-literal` (`PYI061`) ([#&#8203;16044](https://redirect.github.com/astral-sh/ruff/pull/16044))
-   \[`pylint`] Also report when the object isn't a literal (`PLE1310`) ([#&#8203;15985](https://redirect.github.com/astral-sh/ruff/pull/15985))
-   \[`ruff`] Implement `indented-form-feed` (`RUF054`) ([#&#8203;16049](https://redirect.github.com/astral-sh/ruff/pull/16049))
-   \[`ruff`] Skip type definitions for `missing-f-string-syntax` (`RUF027`) ([#&#8203;16054](https://redirect.github.com/astral-sh/ruff/pull/16054))

##### Rule changes

-   \[`flake8-annotations`] Correct syntax for `typing.Union` in suggested return type fixes for `ANN20x` rules ([#&#8203;16025](https://redirect.github.com/astral-sh/ruff/pull/16025))
-   \[`flake8-builtins`] Match upstream module name comparison (`A005`) ([#&#8203;16006](https://redirect.github.com/astral-sh/ruff/pull/16006))
-   \[`flake8-comprehensions`] Detect overshadowed `list`/`set`/`dict`, ignore variadics and named expressions (`C417`) ([#&#8203;15955](https://redirect.github.com/astral-sh/ruff/pull/15955))
-   \[`flake8-pie`] Remove following comma correctly when the unpacked dictionary is empty (`PIE800`) ([#&#8203;16008](https://redirect.github.com/astral-sh/ruff/pull/16008))
-   \[`flake8-simplify`] Only trigger `SIM401` on known dictionaries ([#&#8203;15995](https://redirect.github.com/astral-sh/ruff/pull/15995))
-   \[`pylint`] Do not report calls when object type and argument type mismatch, remove custom escape handling logic (`PLE1310`) ([#&#8203;15984](https://redirect.github.com/astral-sh/ruff/pull/15984))
-   \[`pyupgrade`] Comments within parenthesized value ranges should not affect applicability (`UP040`) ([#&#8203;16027](https://redirect.github.com/astral-sh/ruff/pull/16027))
-   \[`pyupgrade`] Don't introduce invalid syntax when upgrading old-style type aliases with parenthesized multiline values (`UP040`) ([#&#8203;16026](https://redirect.github.com/astral-sh/ruff/pull/16026))
-   \[`pyupgrade`] Ensure we do not rename two type parameters to the same name (`UP049`) ([#&#8203;16038](https://redirect.github.com/astral-sh/ruff/pull/16038))
-   \[`pyupgrade`] \[`ruff`] Don't apply renamings if the new name is shadowed in a scope of one of the references to the binding (`UP049`, `RUF052`) ([#&#8203;16032](https://redirect.github.com/astral-sh/ruff/pull/16032))
-   \[`ruff`] Update `RUF009` to behave similar to `B008` and ignore attributes with immutable types ([#&#8203;16048](https://redirect.github.com/astral-sh/ruff/pull/16048))

##### Server

-   Root exclusions in the server to project root ([#&#8203;16043](https://redirect.github.com/astral-sh/ruff/pull/16043))

##### Bug fixes

-   \[`flake8-datetime`] Ignore `.replace()` calls while looking for `.astimezone` ([#&#8203;16050](https://redirect.github.com/astral-sh/ruff/pull/16050))
-   \[`flake8-type-checking`] Avoid `TC004` false positive where the runtime definition is provided by `__getattr__` ([#&#8203;16052](https://redirect.github.com/astral-sh/ruff/pull/16052))

##### Documentation

-   Improve `ruff-lsp` migration document ([#&#8203;16072](https://redirect.github.com/astral-sh/ruff/pull/16072))
-   Undeprecate `ruff.nativeServer` ([#&#8203;16039](https://redirect.github.com/astral-sh/ruff/pull/16039))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xNjcuMSIsInVwZGF0ZWRJblZlciI6IjM5LjE2Ny4xIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-02-17 03:43_

---

_Merged by @MichaReiser on 2025-02-17 07:21_

---

_Closed by @MichaReiser on 2025-02-17 07:21_

---

_Branch deleted on 2025-02-17 07:21_

---
