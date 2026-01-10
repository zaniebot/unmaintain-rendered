```yaml
number: 20396
title: Update dependency ruff to v0.13.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-09-15T01:29:58Z
updated_at: 2025-09-15T01:44:25Z
url: https://github.com/astral-sh/ruff/pull/20396
synced_at: 2026-01-10T17:40:28Z
```

# Update dependency ruff to v0.13.0

---

_Pull request opened by @renovate on 2025-09-15 01:29_

This PR contains the following updates:

| Package | Change | Age | Confidence |
|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.12.12` -> `==0.13.0` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.13.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.12.12/0.13.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.13.0`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0130)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.12.12...0.13.0)

Check out the [blog post](https://astral.sh/blog/ruff-v0.13.0) for a migration
guide and overview of the changes!

##### Breaking changes

- **Several rules can now add `from __future__ import annotations` automatically**

  `TC001`, `TC002`, `TC003`, `RUF013`, and `UP037` now add `from __future__ import annotations` as part of their fixes when the
  `lint.future-annotations` setting is enabled. This allows the rules to move
  more imports into `TYPE_CHECKING` blocks (`TC001`, `TC002`, and `TC003`),
  use PEP 604 union syntax on Python versions before 3.10 (`RUF013`), and
  unquote more annotations (`UP037`).

- **Full module paths are now used to verify first-party modules**

  Ruff now checks that the full path to a module exists on disk before
  categorizing it as a first-party import. This change makes first-party
  import detection more accurate, helping to avoid false positives on local
  directories with the same name as a third-party dependency, for example. See
  the [FAQ
  section](https://docs.astral.sh/ruff/faq/#how-does-ruff-determine-which-of-my-imports-are-first-party-third-party-etc) on import categorization for more details.

- **Deprecated rules must now be selected by exact rule code**

  Ruff will no longer activate deprecated rules selected by their group name
  or prefix. As noted below, the two remaining deprecated rules were also
  removed in this release, so this won't affect any current rules, but it will
  still affect any deprecations in the future.

- **The deprecated macOS configuration directory fallback has been removed**

  Ruff will no longer look for a user-level configuration file at
  `~/Library/Application Support/ruff/ruff.toml` on macOS. This feature was
  deprecated in v0.5 in favor of using the [XDG
  specification](https://specifications.freedesktop.org/basedir-spec/latest/)
  (usually resolving to `~/.config/ruff/ruff.toml`), like on Linux. The
  fallback and accompanying deprecation warning have now been removed.

##### Removed Rules

The following rules have been removed:

- [`pandas-df-variable-name`](https://docs.astral.sh/ruff/rules/pandas-df-variable-name) (`PD901`)
- [`non-pep604-isinstance`](https://docs.astral.sh/ruff/rules/non-pep604-isinstance) (`UP038`)

##### Stabilization

The following rules have been stabilized and are no longer in preview:

- [`airflow-dag-no-schedule-argument`](https://docs.astral.sh/ruff/rules/airflow-dag-no-schedule-argument)
  (`AIR002`)
- [`airflow3-removal`](https://docs.astral.sh/ruff/rules/airflow3-removal) (`AIR301`)
- [`airflow3-moved-to-provider`](https://docs.astral.sh/ruff/rules/airflow3-moved-to-provider)
  (`AIR302`)
- [`airflow3-suggested-update`](https://docs.astral.sh/ruff/rules/airflow3-suggested-update)
  (`AIR311`)
- [`airflow3-suggested-to-move-to-provider`](https://docs.astral.sh/ruff/rules/airflow3-suggested-to-move-to-provider)
  (`AIR312`)
- [`long-sleep-not-forever`](https://docs.astral.sh/ruff/rules/long-sleep-not-forever) (`ASYNC116`)
- [`f-string-number-format`](https://docs.astral.sh/ruff/rules/f-string-number-format) (`FURB116`)
- [`os-symlink`](https://docs.astral.sh/ruff/rules/os-symlink) (`PTH211`)
- [`generic-not-last-base-class`](https://docs.astral.sh/ruff/rules/generic-not-last-base-class)
  (`PYI059`)
- [`redundant-none-literal`](https://docs.astral.sh/ruff/rules/redundant-none-literal) (`PYI061`)
- [`pytest-raises-ambiguous-pattern`](https://docs.astral.sh/ruff/rules/pytest-raises-ambiguous-pattern)
  (`RUF043`)
- [`unused-unpacked-variable`](https://docs.astral.sh/ruff/rules/unused-unpacked-variable)
  (`RUF059`)
- [`useless-class-metaclass-type`](https://docs.astral.sh/ruff/rules/useless-class-metaclass-type)
  (`UP050`)

The following behaviors have been stabilized:

- [`assert-raises-exception`](https://docs.astral.sh/ruff/rules/assert-raises-exception) (`B017`)
  now checks for direct calls to `unittest.TestCase.assert_raises` and `pytest.raises` instead of
  only the context manager forms.
- [`missing-trailing-comma`](https://docs.astral.sh/ruff/rules/missing-trailing-comma) (`COM812`)
  and [`prohibited-trailing-comma`](https://docs.astral.sh/ruff/rules/prohibited-trailing-comma)
  (`COM819`) now check for trailing commas in PEP 695 type parameter lists.
- [`raw-string-in-exception`](https://docs.astral.sh/ruff/rules/raw-string-in-exception) (`EM101`)
  now also checks for byte strings in exception messages.
- [`invalid-mock-access`](https://docs.astral.sh/ruff/rules/invalid-mock-access) (`PGH005`) now
  checks for `AsyncMock` methods like `not_awaited` in addition to the synchronous variants.
- [`useless-import-alias`](https://docs.astral.sh/ruff/rules/useless-import-alias) (`PLC0414`) no
  longer applies to `__init__.py` files, where it conflicted with one of the suggested fixes for
  [`unused-import`](https://docs.astral.sh/ruff/rules/unused-import) (`F401`).
- [`bidirectional-unicode`](https://docs.astral.sh/ruff/rules/bidirectional-unicode) (`PLE2502`) now
  also checks for U+061C (Arabic Letter Mark).
- The fix for
  [`multiple-with-statements`](https://docs.astral.sh/ruff/rules/multiple-with-statements)
  (`SIM117`) is now marked as always safe.

##### Preview features

- \[`pyupgrade`] Enable `UP043` in stub files ([#&#8203;20027](https://redirect.github.com/astral-sh/ruff/pull/20027))

##### Bug fixes

- \[`pyupgrade`] Apply `UP008` only when the `__class__` cell exists ([#&#8203;19424](https://redirect.github.com/astral-sh/ruff/pull/19424))
- \[`ruff`] Fix empty f-string detection in `in-empty-collection` (`RUF060`) ([#&#8203;20249](https://redirect.github.com/astral-sh/ruff/pull/20249))

##### Server

- Add support for using uv as an alternative formatter backend ([#&#8203;19665](https://redirect.github.com/astral-sh/ruff/pull/19665))

##### Documentation

- \[`pep8-naming`] Fix formatting of `__all__` (`N816`) ([#&#8203;20301](https://redirect.github.com/astral-sh/ruff/pull/20301))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS45Ny4xMCIsInVwZGF0ZWRJblZlciI6IjQxLjk3LjEwIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-09-15 01:29_

---

_Merged by @charliermarsh on 2025-09-15 01:44_

---

_Closed by @charliermarsh on 2025-09-15 01:44_

---

_Branch deleted on 2025-09-15 01:44_

---
