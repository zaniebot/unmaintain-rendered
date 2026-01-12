```yaml
number: 20622
title: Update dependency ruff to v0.13.2
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-09-29T02:10:21Z
updated_at: 2025-09-29T06:59:52Z
url: https://github.com/astral-sh/ruff/pull/20622
synced_at: 2026-01-12T15:57:06Z
```

# Update dependency ruff to v0.13.2

---

_@renovate_

Coming soon: The Renovate bot (GitHub App) will be renamed to Mend. PRs from Renovate will soon appear from 'Mend'. Learn more [here](https://redirect.github.com/renovatebot/renovate/discussions/37842).

This PR contains the following updates:

| Package | Change | Age | Confidence |
|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.13.1` -> `==0.13.2` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.13.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.13.1/0.13.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.13.2`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0132)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.13.1...0.13.2)

Released on 2025-09-25.

##### Preview features

- \[`flake8-async`] Implement `blocking-path-method` (`ASYNC240`) ([#&#8203;20264](https://redirect.github.com/astral-sh/ruff/pull/20264))
- \[`flake8-bugbear`] Implement `map-without-explicit-strict` (`B912`) ([#&#8203;20429](https://redirect.github.com/astral-sh/ruff/pull/20429))
- \[`flake8-bultins`] Detect class-scope builtin shadowing in decorators, default args, and attribute initializers (`A003`) ([#&#8203;20178](https://redirect.github.com/astral-sh/ruff/pull/20178))
- \[`ruff`] Implement `logging-eager-conversion` (`RUF065`) ([#&#8203;19942](https://redirect.github.com/astral-sh/ruff/pull/19942))
- Include `.pyw` files by default when linting and formatting ([#&#8203;20458](https://redirect.github.com/astral-sh/ruff/pull/20458))

##### Bug fixes

- Deduplicate input paths ([#&#8203;20105](https://redirect.github.com/astral-sh/ruff/pull/20105))
- \[`flake8-comprehensions`] Preserve trailing commas for single-element lists (`C409`) ([#&#8203;19571](https://redirect.github.com/astral-sh/ruff/pull/19571))
- \[`flake8-pyi`] Avoid syntax error from conflict with `PIE790` (`PYI021`) ([#&#8203;20010](https://redirect.github.com/astral-sh/ruff/pull/20010))
- \[`flake8-simplify`] Correct fix for positive `maxsplit` without separator (`SIM905`) ([#&#8203;20056](https://redirect.github.com/astral-sh/ruff/pull/20056))
- \[`pyupgrade`] Fix `UP008` not to apply when `__class__` is a local variable ([#&#8203;20497](https://redirect.github.com/astral-sh/ruff/pull/20497))
- \[`ruff`] Fix `B004` to skip invalid `hasattr`/`getattr` calls ([#&#8203;20486](https://redirect.github.com/astral-sh/ruff/pull/20486))
- \[`ruff`] Replace `-nan` with `nan` when using the value to construct a `Decimal` (`FURB164` ) ([#&#8203;20391](https://redirect.github.com/astral-sh/ruff/pull/20391))

##### Documentation

- Add 'Finding ways to help' to CONTRIBUTING.md ([#&#8203;20567](https://redirect.github.com/astral-sh/ruff/pull/20567))
- Update import path to `ruff-wasm-web` ([#&#8203;20539](https://redirect.github.com/astral-sh/ruff/pull/20539))
- \[`flake8-bandit`] Clarify the supported hashing functions (`S324`) ([#&#8203;20534](https://redirect.github.com/astral-sh/ruff/pull/20534))

##### Other changes

- \[`playground`] Allow hover quick fixes to appear for overlapping diagnostics ([#&#8203;20527](https://redirect.github.com/astral-sh/ruff/pull/20527))
- \[`playground`] Fix non‑BMP code point handling in quick fixes and markers ([#&#8203;20526](https://redirect.github.com/astral-sh/ruff/pull/20526))

##### Contributors

- [@&#8203;BurntSushi](https://redirect.github.com/BurntSushi)
- [@&#8203;mtshiba](https://redirect.github.com/mtshiba)
- [@&#8203;second-ed](https://redirect.github.com/second-ed)
- [@&#8203;danparizher](https://redirect.github.com/danparizher)
- [@&#8203;ShikChen](https://redirect.github.com/ShikChen)
- [@&#8203;PieterCK](https://redirect.github.com/PieterCK)
- [@&#8203;GDYendell](https://redirect.github.com/GDYendell)
- [@&#8203;RazerM](https://redirect.github.com/RazerM)
- [@&#8203;TaKO8Ki](https://redirect.github.com/TaKO8Ki)
- [@&#8203;amyreese](https://redirect.github.com/amyreese)
- [@&#8203;ntbre](https://redirect.github.com/ntBre)
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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS4xMzEuOSIsInVwZGF0ZWRJblZlciI6IjQxLjEzMS45IiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-09-29 02:10_

---

_Merged by @sharkdp on 2025-09-29 06:59_

---

_Closed by @sharkdp on 2025-09-29 06:59_

---

_Branch deleted on 2025-09-29 06:59_

---
