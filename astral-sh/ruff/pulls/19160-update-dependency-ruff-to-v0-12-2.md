```yaml
number: 19160
title: Update dependency ruff to v0.12.2
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-07-07T02:46:52Z
updated_at: 2025-07-07T03:24:37Z
url: https://github.com/astral-sh/ruff/pull/19160
synced_at: 2026-01-10T18:33:12Z
```

# Update dependency ruff to v0.12.2

---

_Pull request opened by @renovate on 2025-07-07 02:46_

This PR contains the following updates:

| Package | Change | Age | Confidence |
|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.12.1` -> `==0.12.2` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.12.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.12.1/0.12.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.12.2`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0122)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.12.1...0.12.2)

##### Preview features

- \[`flake8-pyi`] Expand `Optional[A]` to `A | None` (`PYI016`) ([#&#8203;18572](https://redirect.github.com/astral-sh/ruff/pull/18572))
- \[`pyupgrade`] Mark `UP008` fix safe if no comments are in range ([#&#8203;18683](https://redirect.github.com/astral-sh/ruff/pull/18683))

##### Bug fixes

- \[`flake8-comprehensions`] Fix `C420` to prepend whitespace when needed ([#&#8203;18616](https://redirect.github.com/astral-sh/ruff/pull/18616))
- \[`perflint`] Fix `PERF403` panic on attribute or subscription loop variable ([#&#8203;19042](https://redirect.github.com/astral-sh/ruff/pull/19042))
- \[`pydocstyle`] Fix `D413` infinite loop for parenthesized docstring ([#&#8203;18930](https://redirect.github.com/astral-sh/ruff/pull/18930))
- \[`pylint`] Fix `PLW0108` autofix introducing a syntax error when the lambda's body contains an assignment expression ([#&#8203;18678](https://redirect.github.com/astral-sh/ruff/pull/18678))
- \[`refurb`] Fix false positive on empty tuples (`FURB168`) ([#&#8203;19058](https://redirect.github.com/astral-sh/ruff/pull/19058))
- \[`ruff`] Allow more `field` calls from `attrs` (`RUF009`) ([#&#8203;19021](https://redirect.github.com/astral-sh/ruff/pull/19021))
- \[`ruff`] Fix syntax error introduced for an empty string followed by a u-prefixed string (`UP025`) ([#&#8203;18899](https://redirect.github.com/astral-sh/ruff/pull/18899))

##### Rule changes

- \[`flake8-executable`] Allow `uvx` in shebang line (`EXE003`) ([#&#8203;18967](https://redirect.github.com/astral-sh/ruff/pull/18967))
- \[`pandas`] Avoid flagging `PD002` if `pandas` is not imported ([#&#8203;18963](https://redirect.github.com/astral-sh/ruff/pull/18963))
- \[`pyupgrade`] Avoid PEP-604 unions with `typing.NamedTuple` (`UP007`, `UP045`) ([#&#8203;18682](https://redirect.github.com/astral-sh/ruff/pull/18682))

##### Documentation

- Document link between `import-outside-top-level (PLC0415)` and `lint.flake8-tidy-imports.banned-module-level-imports` ([#&#8203;18733](https://redirect.github.com/astral-sh/ruff/pull/18733))
- Fix description of the `format.skip-magic-trailing-comma` example ([#&#8203;19095](https://redirect.github.com/astral-sh/ruff/pull/19095))
- \[`airflow`] Make `AIR302` example error out-of-the-box ([#&#8203;18988](https://redirect.github.com/astral-sh/ruff/pull/18988))
- \[`airflow`] Make `AIR312` example error out-of-the-box ([#&#8203;18989](https://redirect.github.com/astral-sh/ruff/pull/18989))
- \[`flake8-annotations`] Make `ANN401` example error out-of-the-box ([#&#8203;18974](https://redirect.github.com/astral-sh/ruff/pull/18974))
- \[`flake8-async`] Make `ASYNC100` example error out-of-the-box ([#&#8203;18993](https://redirect.github.com/astral-sh/ruff/pull/18993))
- \[`flake8-async`] Make `ASYNC105` example error out-of-the-box ([#&#8203;19002](https://redirect.github.com/astral-sh/ruff/pull/19002))
- \[`flake8-async`] Make `ASYNC110` example error out-of-the-box ([#&#8203;18975](https://redirect.github.com/astral-sh/ruff/pull/18975))
- \[`flake8-async`] Make `ASYNC210` example error out-of-the-box ([#&#8203;18977](https://redirect.github.com/astral-sh/ruff/pull/18977))
- \[`flake8-async`] Make `ASYNC220`, `ASYNC221`, and `ASYNC222` examples error out-of-the-box ([#&#8203;18978](https://redirect.github.com/astral-sh/ruff/pull/18978))
- \[`flake8-async`] Make `ASYNC251` example error out-of-the-box ([#&#8203;18990](https://redirect.github.com/astral-sh/ruff/pull/18990))
- \[`flake8-bandit`] Make `S201` example error out-of-the-box ([#&#8203;19017](https://redirect.github.com/astral-sh/ruff/pull/19017))
- \[`flake8-bandit`] Make `S604` and `S609` examples error out-of-the-box ([#&#8203;19049](https://redirect.github.com/astral-sh/ruff/pull/19049))
- \[`flake8-bugbear`] Make `B028` example error out-of-the-box ([#&#8203;19054](https://redirect.github.com/astral-sh/ruff/pull/19054))
- \[`flake8-bugbear`] Make `B911` example error out-of-the-box ([#&#8203;19051](https://redirect.github.com/astral-sh/ruff/pull/19051))
- \[`flake8-datetimez`] Make `DTZ011` example error out-of-the-box ([#&#8203;19055](https://redirect.github.com/astral-sh/ruff/pull/19055))
- \[`flake8-datetimez`] Make `DTZ901` example error out-of-the-box ([#&#8203;19056](https://redirect.github.com/astral-sh/ruff/pull/19056))
- \[`flake8-pyi`] Make `PYI032` example error out-of-the-box ([#&#8203;19061](https://redirect.github.com/astral-sh/ruff/pull/19061))
- \[`flake8-pyi`] Make example error out-of-the-box (`PYI014`, `PYI015`) ([#&#8203;19097](https://redirect.github.com/astral-sh/ruff/pull/19097))
- \[`flake8-pyi`] Make example error out-of-the-box (`PYI042`) ([#&#8203;19101](https://redirect.github.com/astral-sh/ruff/pull/19101))
- \[`flake8-pyi`] Make example error out-of-the-box (`PYI059`) ([#&#8203;19080](https://redirect.github.com/astral-sh/ruff/pull/19080))
- \[`flake8-pyi`] Make example error out-of-the-box (`PYI062`) ([#&#8203;19079](https://redirect.github.com/astral-sh/ruff/pull/19079))
- \[`flake8-pytest-style`] Make example error out-of-the-box (`PT023`) ([#&#8203;19104](https://redirect.github.com/astral-sh/ruff/pull/19104))
- \[`flake8-pytest-style`] Make example error out-of-the-box (`PT030`) ([#&#8203;19105](https://redirect.github.com/astral-sh/ruff/pull/19105))
- \[`flake8-quotes`] Make example error out-of-the-box (`Q003`) ([#&#8203;19106](https://redirect.github.com/astral-sh/ruff/pull/19106))
- \[`flake8-simplify`] Make example error out-of-the-box (`SIM110`) ([#&#8203;19113](https://redirect.github.com/astral-sh/ruff/pull/19113))
- \[`flake8-simplify`] Make example error out-of-the-box (`SIM113`) ([#&#8203;19109](https://redirect.github.com/astral-sh/ruff/pull/19109))
- \[`flake8-simplify`] Make example error out-of-the-box (`SIM401`) ([#&#8203;19110](https://redirect.github.com/astral-sh/ruff/pull/19110))
- \[`pyflakes`] Fix backslash in docs (`F621`) ([#&#8203;19098](https://redirect.github.com/astral-sh/ruff/pull/19098))
- \[`pylint`] Fix `PLC0415` example ([#&#8203;18970](https://redirect.github.com/astral-sh/ruff/pull/18970))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS4xNy4yIiwidXBkYXRlZEluVmVyIjoiNDEuMTcuMiIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-07-07 02:46_

---

_Merged by @dhruvmanila on 2025-07-07 03:24_

---

_Closed by @dhruvmanila on 2025-07-07 03:24_

---

_Branch deleted on 2025-07-07 03:24_

---
