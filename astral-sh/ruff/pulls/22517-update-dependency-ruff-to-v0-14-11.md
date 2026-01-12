```yaml
number: 22517
title: Update dependency ruff to v0.14.11
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2026-01-12T07:39:04Z
updated_at: 2026-01-12T07:52:16Z
url: https://github.com/astral-sh/ruff/pull/22517
synced_at: 2026-01-12T08:53:00Z
```

# Update dependency ruff to v0.14.11

---

_Pull request opened by @renovate on 2026-01-12 07:39_

This PR contains the following updates:

| Package | Change | [Age](https://docs.renovatebot.com/merge-confidence/) | [Confidence](https://docs.renovatebot.com/merge-confidence/) |
|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.14.10` → `==0.14.11` | ![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.14.11?slim=true) | ![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.14.10/0.14.11?slim=true) |

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.14.11`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#01411)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.14.10...0.14.11)

Released on 2026-01-08.

##### Preview features

- Consolidate diagnostics for matched disable/enable suppression comments ([#&#8203;22099](https://redirect.github.com/astral-sh/ruff/pull/22099))
- Report diagnostics for invalid/unmatched range suppression comments ([#&#8203;21908](https://redirect.github.com/astral-sh/ruff/pull/21908))
- \[`airflow`] Passing positional argument into `airflow.lineage.hook.HookLineageCollector.create_asset` is not allowed (`AIR303`) ([#&#8203;22046](https://redirect.github.com/astral-sh/ruff/pull/22046))
- \[`refurb`] Mark `FURB192` fix as always unsafe ([#&#8203;22210](https://redirect.github.com/astral-sh/ruff/pull/22210))
- \[`ruff`] Add `non-empty-init-module` (`RUF067`) ([#&#8203;22143](https://redirect.github.com/astral-sh/ruff/pull/22143))

##### Bug fixes

- Fix GitHub format for multi-line diagnostics ([#&#8203;22108](https://redirect.github.com/astral-sh/ruff/pull/22108))
- \[`flake8-unused-arguments`] Mark `**kwargs` in `TypeVar` as used (`ARG001`) ([#&#8203;22214](https://redirect.github.com/astral-sh/ruff/pull/22214))

##### Rule changes

- Add `help:` subdiagnostics for several Ruff rules that can sometimes appear to disagree with `ty` ([#&#8203;22331](https://redirect.github.com/astral-sh/ruff/pull/22331))
- \[`pylint`] Demote `PLW1510` fix to display-only ([#&#8203;22318](https://redirect.github.com/astral-sh/ruff/pull/22318))
- \[`pylint`] Ignore identical members (`PLR1714`) ([#&#8203;22220](https://redirect.github.com/astral-sh/ruff/pull/22220))
- \[`pylint`] Improve diagnostic range for `PLC0206` ([#&#8203;22312](https://redirect.github.com/astral-sh/ruff/pull/22312))
- \[`ruff`] Improve fix title for `RUF102` invalid rule code ([#&#8203;22100](https://redirect.github.com/astral-sh/ruff/pull/22100))
- \[`flake8-simplify`]: Avoid unnecessary builtins import for `SIM105` ([#&#8203;22358](https://redirect.github.com/astral-sh/ruff/pull/22358))

##### Configuration

- Allow Python 3.15 as valid `target-version` value in preview ([#&#8203;22419](https://redirect.github.com/astral-sh/ruff/pull/22419))
- Check `required-version` before parsing rules ([#&#8203;22410](https://redirect.github.com/astral-sh/ruff/pull/22410))
- Include configured `src` directories when resolving graphs ([#&#8203;22451](https://redirect.github.com/astral-sh/ruff/pull/22451))

##### Documentation

- Update `T201` suggestion to not use root logger to satisfy `LOG015` ([#&#8203;22059](https://redirect.github.com/astral-sh/ruff/pull/22059))
- Fix `iter` example in unsafe fixes doc ([#&#8203;22118](https://redirect.github.com/astral-sh/ruff/pull/22118))
- \[`flake8_print`] better suggestion for `basicConfig` in `T201` docs ([#&#8203;22101](https://redirect.github.com/astral-sh/ruff/pull/22101))
- \[`pylint`] Restore the fix safety docs for `PLW0133` ([#&#8203;22211](https://redirect.github.com/astral-sh/ruff/pull/22211))
- Fix Jupyter notebook discovery info for editors ([#&#8203;22447](https://redirect.github.com/astral-sh/ruff/pull/22447))

##### Contributors

- [@&#8203;charliermarsh](https://redirect.github.com/charliermarsh)
- [@&#8203;ntBre](https://redirect.github.com/ntBre)
- [@&#8203;cenviity](https://redirect.github.com/cenviity)
- [@&#8203;njhearp](https://redirect.github.com/njhearp)
- [@&#8203;cbachhuber](https://redirect.github.com/cbachhuber)
- [@&#8203;jelle-openai](https://redirect.github.com/jelle-openai)
- [@&#8203;AlexWaygood](https://redirect.github.com/AlexWaygood)
- [@&#8203;ValdonVitija](https://redirect.github.com/ValdonVitija)
- [@&#8203;BurntSushi](https://redirect.github.com/BurntSushi)
- [@&#8203;Jkhall81](https://redirect.github.com/Jkhall81)
- [@&#8203;PeterJCLaw](https://redirect.github.com/PeterJCLaw)
- [@&#8203;harupy](https://redirect.github.com/harupy)
- [@&#8203;amyreese](https://redirect.github.com/amyreese)
- [@&#8203;sjyangkevin](https://redirect.github.com/sjyangkevin)
- [@&#8203;woodruffw](https://redirect.github.com/woodruffw)

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi43NC41IiwidXBkYXRlZEluVmVyIjoiNDIuNzQuNSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2026-01-12 07:39_

---

_Merged by @MichaReiser on 2026-01-12 07:52_

---

_Closed by @MichaReiser on 2026-01-12 07:52_

---

_Branch deleted on 2026-01-12 07:52_

---
