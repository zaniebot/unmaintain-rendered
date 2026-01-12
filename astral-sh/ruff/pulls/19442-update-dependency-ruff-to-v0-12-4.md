```yaml
number: 19442
title: Update dependency ruff to v0.12.4
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-07-21T01:05:09Z
updated_at: 2025-07-21T06:32:11Z
url: https://github.com/astral-sh/ruff/pull/19442
synced_at: 2026-01-12T15:56:39Z
```

# Update dependency ruff to v0.12.4

---

_@renovate_

This PR contains the following updates:

| Package | Change | Age | Confidence |
|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.12.3` -> `==0.12.4` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.12.4?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.12.3/0.12.4?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.12.4`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0124)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.12.3...0.12.4)

##### Preview features

- \[`flake8-type-checking`, `pyupgrade`, `ruff`] Add `from __future__ import annotations` when it would allow new fixes (`TC001`, `TC002`, `TC003`, `UP037`, `RUF013`) ([#&#8203;19100](https://redirect.github.com/astral-sh/ruff/pull/19100))
- \[`flake8-use-pathlib`] Add autofix for `PTH109` ([#&#8203;19245](https://redirect.github.com/astral-sh/ruff/pull/19245))
- \[`pylint`] Detect indirect `pathlib.Path` usages for `unspecified-encoding` (`PLW1514`) ([#&#8203;19304](https://redirect.github.com/astral-sh/ruff/pull/19304))

##### Bug fixes

- \[`flake8-bugbear`] Fix `B017` false negatives for keyword exception arguments ([#&#8203;19217](https://redirect.github.com/astral-sh/ruff/pull/19217))
- \[`flake8-use-pathlib`] Fix false negative on direct `Path()` instantiation (`PTH210`) ([#&#8203;19388](https://redirect.github.com/astral-sh/ruff/pull/19388))
- \[`flake8-django`] Fix `DJ008` false positive for abstract models with type-annotated `abstract` field ([#&#8203;19221](https://redirect.github.com/astral-sh/ruff/pull/19221))
- \[`isort`] Fix `I002` import insertion after docstring with multiple string statements ([#&#8203;19222](https://redirect.github.com/astral-sh/ruff/pull/19222))
- \[`isort`] Treat form feed as valid whitespace before a semicolon ([#&#8203;19343](https://redirect.github.com/astral-sh/ruff/pull/19343))
- \[`pydoclint`] Fix `SyntaxError` from fixes with line continuations (`D201`, `D202`) ([#&#8203;19246](https://redirect.github.com/astral-sh/ruff/pull/19246))
- \[`refurb`] `FURB164` fix should validate arguments and should usually be marked unsafe ([#&#8203;19136](https://redirect.github.com/astral-sh/ruff/pull/19136))

##### Rule changes

- \[`flake8-use-pathlib`] Skip single dots for `invalid-pathlib-with-suffix` (`PTH210`) on versions >= 3.14 ([#&#8203;19331](https://redirect.github.com/astral-sh/ruff/pull/19331))
- \[`pep8_naming`] Avoid false positives on standard library functions with uppercase names (`N802`) ([#&#8203;18907](https://redirect.github.com/astral-sh/ruff/pull/18907))
- \[`pycodestyle`] Handle brace escapes for t-strings in logical lines ([#&#8203;19358](https://redirect.github.com/astral-sh/ruff/pull/19358))
- \[`pylint`] Extend invalid string character rules to include t-strings ([#&#8203;19355](https://redirect.github.com/astral-sh/ruff/pull/19355))
- \[`ruff`] Allow `strict` kwarg when checking for `starmap-zip` (`RUF058`) in Python 3.14+ ([#&#8203;19333](https://redirect.github.com/astral-sh/ruff/pull/19333))

##### Documentation

- \[`flake8-type-checking`] Make `TC010` docs example more realistic ([#&#8203;19356](https://redirect.github.com/astral-sh/ruff/pull/19356))
- Make more documentation examples error out-of-the-box ([#&#8203;19288](https://redirect.github.com/astral-sh/ruff/pull/19288),[#&#8203;19272](https://redirect.github.com/astral-sh/ruff/pull/19272),[#&#8203;19291](https://redirect.github.com/astral-sh/ruff/pull/19291),[#&#8203;19296](https://redirect.github.com/astral-sh/ruff/pull/19296),[#&#8203;19292](https://redirect.github.com/astral-sh/ruff/pull/19292),[#&#8203;19295](https://redirect.github.com/astral-sh/ruff/pull/19295),[#&#8203;19297](https://redirect.github.com/astral-sh/ruff/pull/19297),[#&#8203;19309](https://redirect.github.com/astral-sh/ruff/pull/19309))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS4yMy4yIiwidXBkYXRlZEluVmVyIjoiNDEuMjMuMiIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-07-21 01:05_

---

_Merged by @MichaReiser on 2025-07-21 06:32_

---

_Closed by @MichaReiser on 2025-07-21 06:32_

---

_Branch deleted on 2025-07-21 06:32_

---
