```yaml
number: 17839
title: Update dependency ruff to v0.11.8
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-05-05T03:14:13Z
updated_at: 2025-05-05T05:29:37Z
url: https://github.com/astral-sh/ruff/pull/17839
synced_at: 2026-01-12T15:56:06Z
```

# Update dependency ruff to v0.11.8

---

_@renovate_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.11.7` -> `==0.11.8` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.11.8?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.11.8?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.11.7/0.11.8?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.11.7/0.11.8?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.11.8`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0118)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.11.7...0.11.8)

##### Preview features

-   \[`airflow`] Apply auto fixes to cases where the names have changed in Airflow 3 (`AIR302`, `AIR311`) ([#&#8203;17553](https://redirect.github.com/astral-sh/ruff/pull/17553), [#&#8203;17570](https://redirect.github.com/astral-sh/ruff/pull/17570), [#&#8203;17571](https://redirect.github.com/astral-sh/ruff/pull/17571))
-   \[`airflow`] Extend `AIR301` rule ([#&#8203;17598](https://redirect.github.com/astral-sh/ruff/pull/17598))
-   \[`airflow`] Update existing `AIR302` rules with better suggestions ([#&#8203;17542](https://redirect.github.com/astral-sh/ruff/pull/17542))
-   \[`refurb`] Mark fix as safe for `readlines-in-for` (`FURB129`) ([#&#8203;17644](https://redirect.github.com/astral-sh/ruff/pull/17644))
-   \[syntax-errors] `nonlocal` declaration at module level ([#&#8203;17559](https://redirect.github.com/astral-sh/ruff/pull/17559))
-   \[syntax-errors] Detect single starred expression assignment `x = *y` ([#&#8203;17624](https://redirect.github.com/astral-sh/ruff/pull/17624))

##### Bug fixes

-   \[`flake8-pyi`] Ensure `Literal[None,] | Literal[None,]` is not autofixed to `None | None` (`PYI061`) ([#&#8203;17659](https://redirect.github.com/astral-sh/ruff/pull/17659))
-   \[`flake8-use-pathlib`] Avoid suggesting `Path.iterdir()` for `os.listdir` with file descriptor (`PTH208`) ([#&#8203;17715](https://redirect.github.com/astral-sh/ruff/pull/17715))
-   \[`flake8-use-pathlib`] Fix `PTH104` false positive when `rename` is passed a file descriptor ([#&#8203;17712](https://redirect.github.com/astral-sh/ruff/pull/17712))
-   \[`flake8-use-pathlib`] Fix `PTH116` false positive when `stat` is passed a file descriptor ([#&#8203;17709](https://redirect.github.com/astral-sh/ruff/pull/17709))
-   \[`flake8-use-pathlib`] Fix `PTH123` false positive when `open` is passed a file descriptor from a function call ([#&#8203;17705](https://redirect.github.com/astral-sh/ruff/pull/17705))
-   \[`pycodestyle`] Fix duplicated diagnostic in `E712` ([#&#8203;17651](https://redirect.github.com/astral-sh/ruff/pull/17651))
-   \[`pylint`] Detect `global` declarations in module scope (`PLE0118`) ([#&#8203;17411](https://redirect.github.com/astral-sh/ruff/pull/17411))
-   \[syntax-errors] Make `async-comprehension-in-sync-comprehension` more specific ([#&#8203;17460](https://redirect.github.com/astral-sh/ruff/pull/17460))

##### Configuration

-   Add option to disable `typing_extensions` imports ([#&#8203;17611](https://redirect.github.com/astral-sh/ruff/pull/17611))

##### Documentation

-   Fix example syntax for the `lint.pydocstyle.ignore-var-parameters` option ([#&#8203;17740](https://redirect.github.com/astral-sh/ruff/pull/17740))
-   Add fix safety sections (`ASYNC116`, `FLY002`, `D200`, `RUF005`, `RUF017`, `RUF027`, `RUF028`, `RUF057`) ([#&#8203;17497](https://redirect.github.com/astral-sh/ruff/pull/17497), [#&#8203;17496](https://redirect.github.com/astral-sh/ruff/pull/17496), [#&#8203;17502](https://redirect.github.com/astral-sh/ruff/pull/17502), [#&#8203;17484](https://redirect.github.com/astral-sh/ruff/pull/17484), [#&#8203;17480](https://redirect.github.com/astral-sh/ruff/pull/17480), [#&#8203;17485](https://redirect.github.com/astral-sh/ruff/pull/17485), [#&#8203;17722](https://redirect.github.com/astral-sh/ruff/pull/17722), [#&#8203;17483](https://redirect.github.com/astral-sh/ruff/pull/17483))

##### Other changes

-   Add Python 3.14 to configuration options ([#&#8203;17647](https://redirect.github.com/astral-sh/ruff/pull/17647))
-   Make syntax error for unparenthesized except tuples version specific to before 3.14 ([#&#8203;17660](https://redirect.github.com/astral-sh/ruff/pull/17660))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4yNjQuMCIsInVwZGF0ZWRJblZlciI6IjM5LjI2NC4wIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-05-05 03:14_

---

_Merged by @MichaReiser on 2025-05-05 05:29_

---

_Closed by @MichaReiser on 2025-05-05 05:29_

---

_Branch deleted on 2025-05-05 05:29_

---
