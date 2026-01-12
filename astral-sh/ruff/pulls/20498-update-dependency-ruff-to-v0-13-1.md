```yaml
number: 20498
title: Update dependency ruff to v0.13.1
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-09-22T01:15:43Z
updated_at: 2025-09-22T07:02:08Z
url: https://github.com/astral-sh/ruff/pull/20498
synced_at: 2026-01-12T15:57:03Z
```

# Update dependency ruff to v0.13.1

---

_@renovate_

Coming soon: The Renovate bot (GitHub App) will be renamed to Mend. PRs from Renovate will soon appear from 'Mend'. Learn more [here](https://redirect.github.com/renovatebot/renovate/discussions/37842).

This PR contains the following updates:

| Package | Change | Age | Confidence |
|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.13.0` -> `==0.13.1` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.13.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.13.0/0.13.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.13.1`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0131)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.13.0...0.13.1)

Released on 2025-09-18.

##### Preview features

- \[`flake8-simplify`] Detect unnecessary `None` default for additional key expression types (`SIM910`) ([#&#8203;20343](https://redirect.github.com/astral-sh/ruff/pull/20343))
- \[`flake8-use-pathlib`] Add fix for `PTH123` ([#&#8203;20169](https://redirect.github.com/astral-sh/ruff/pull/20169))
- \[`flake8-use-pathlib`] Fix `PTH101`, `PTH104`, `PTH105`, `PTH121` fixes ([#&#8203;20143](https://redirect.github.com/astral-sh/ruff/pull/20143))
- \[`flake8-use-pathlib`] Make `PTH111` fix unsafe because it can change behavior ([#&#8203;20215](https://redirect.github.com/astral-sh/ruff/pull/20215))
- \[`pycodestyle`] Fix `E301` to only trigger for functions immediately within a class ([#&#8203;19768](https://redirect.github.com/astral-sh/ruff/pull/19768))
- \[`refurb`] Mark `single-item-membership-test` fix as always unsafe (`FURB171`) ([#&#8203;20279](https://redirect.github.com/astral-sh/ruff/pull/20279))

##### Bug fixes

- Handle t-strings for token-based rules and suppression comments ([#&#8203;20357](https://redirect.github.com/astral-sh/ruff/pull/20357))
- \[`flake8-bandit`] Fix truthiness: dict-only `**` displays not truthy for `shell` (`S602`, `S604`, `S609`) ([#&#8203;20177](https://redirect.github.com/astral-sh/ruff/pull/20177))
- \[`flake8-simplify`] Fix diagnostic to show correct method name for `str.rsplit` calls (`SIM905`) ([#&#8203;20459](https://redirect.github.com/astral-sh/ruff/pull/20459))
- \[`flynt`] Use triple quotes for joined raw strings with newlines (`FLY002`) ([#&#8203;20197](https://redirect.github.com/astral-sh/ruff/pull/20197))
- \[`pyupgrade`] Fix false positive when class name is shadowed by local variable (`UP008`) ([#&#8203;20427](https://redirect.github.com/astral-sh/ruff/pull/20427))
- \[`pyupgrade`] Prevent infinite loop with `I002` and `UP026` ([#&#8203;20327](https://redirect.github.com/astral-sh/ruff/pull/20327))
- \[`ruff`] Recognize t-strings, generators, and lambdas in `invalid-index-type` (`RUF016`) ([#&#8203;20213](https://redirect.github.com/astral-sh/ruff/pull/20213))

##### Rule changes

- \[`RUF102`] Respect rule redirects in invalid rule code detection ([#&#8203;20245](https://redirect.github.com/astral-sh/ruff/pull/20245))
- \[`flake8-bugbear`] Mark the fix for `unreliable-callable-check` as always unsafe (`B004`) ([#&#8203;20318](https://redirect.github.com/astral-sh/ruff/pull/20318))
- \[`ruff`] Allow dataclass attribute value instantiation from nested frozen dataclass (`RUF009`) ([#&#8203;20352](https://redirect.github.com/astral-sh/ruff/pull/20352))

##### CLI

- Add fixes to `output-format=sarif` ([#&#8203;20300](https://redirect.github.com/astral-sh/ruff/pull/20300))
- Treat panics as fatal diagnostics, sort panics last ([#&#8203;20258](https://redirect.github.com/astral-sh/ruff/pull/20258))

##### Documentation

- \[`ruff`] Add `analyze.string-imports-min-dots` to settings ([#&#8203;20375](https://redirect.github.com/astral-sh/ruff/pull/20375))
- Update README.md with Albumentations new repository URL ([#&#8203;20415](https://redirect.github.com/astral-sh/ruff/pull/20415))

##### Other changes

- Bump MSRV to Rust 1.88 ([#&#8203;20470](https://redirect.github.com/astral-sh/ruff/pull/20470))
- Enable inline noqa for multiline strings in playground ([#&#8203;20442](https://redirect.github.com/astral-sh/ruff/pull/20442))

##### Contributors

- [@&#8203;chirizxc](https://redirect.github.com/chirizxc)
- [@&#8203;danparizher](https://redirect.github.com/danparizher)
- [@&#8203;IDrokin117](https://redirect.github.com/IDrokin117)
- [@&#8203;amyreese](https://redirect.github.com/amyreese)
- [@&#8203;AlexWaygood](https://redirect.github.com/AlexWaygood)
- [@&#8203;dylwil3](https://redirect.github.com/dylwil3)
- [@&#8203;njhearp](https://redirect.github.com/njhearp)
- [@&#8203;woodruffw](https://redirect.github.com/woodruffw)
- [@&#8203;dcreager](https://redirect.github.com/dcreager)
- [@&#8203;TaKO8Ki](https://redirect.github.com/TaKO8Ki)
- [@&#8203;BurntSushi](https://redirect.github.com/BurntSushi)
- [@&#8203;salahelfarissi](https://redirect.github.com/salahelfarissi)
- [@&#8203;MichaReiser](https://redirect.github.com/MichaReiser)

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

_Label `internal` added by @renovate[bot] on 2025-09-22 01:15_

---

_Merged by @MichaReiser on 2025-09-22 07:02_

---

_Closed by @MichaReiser on 2025-09-22 07:02_

---

_Branch deleted on 2025-09-22 07:02_

---
