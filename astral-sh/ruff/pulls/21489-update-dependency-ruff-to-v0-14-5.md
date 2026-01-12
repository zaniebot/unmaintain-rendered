```yaml
number: 21489
title: Update dependency ruff to v0.14.5
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-11-17T01:58:39Z
updated_at: 2025-11-17T05:23:24Z
url: https://github.com/astral-sh/ruff/pull/21489
synced_at: 2026-01-12T15:57:25Z
```

# Update dependency ruff to v0.14.5

---

_@renovate_

This PR contains the following updates:

| Package | Change | Age | Confidence |
|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.14.4` -> `==0.14.5` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.14.5?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.14.4/0.14.5?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.14.5`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0145)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.14.4...0.14.5)

Released on 2025-11-13.

##### Preview features

- \[`flake8-simplify`] Apply `SIM113` when index variable is of type `int` ([#&#8203;21395](https://redirect.github.com/astral-sh/ruff/pull/21395))
- \[`pydoclint`] Fix false positive when Sphinx directives follow a "Raises" section (`DOC502`) ([#&#8203;20535](https://redirect.github.com/astral-sh/ruff/pull/20535))
- \[`pydoclint`] Support NumPy-style comma-separated parameters (`DOC102`) ([#&#8203;20972](https://redirect.github.com/astral-sh/ruff/pull/20972))
- \[`refurb`] Auto-fix annotated assignments (`FURB101`) ([#&#8203;21278](https://redirect.github.com/astral-sh/ruff/pull/21278))
- \[`ruff`] Ignore `str()` when not used for simple conversion (`RUF065`) ([#&#8203;21330](https://redirect.github.com/astral-sh/ruff/pull/21330))

##### Bug fixes

- Fix syntax error false positive on alternative `match` patterns ([#&#8203;21362](https://redirect.github.com/astral-sh/ruff/pull/21362))
- \[`flake8-simplify`] Fix false positive for iterable initializers with generator arguments (`SIM222`) ([#&#8203;21187](https://redirect.github.com/astral-sh/ruff/pull/21187))
- \[`pyupgrade`] Fix false positive on relative imports from local `.builtins` module (`UP029`) ([#&#8203;21309](https://redirect.github.com/astral-sh/ruff/pull/21309))
- \[`pyupgrade`] Consistently set the deprecated tag (`UP035`) ([#&#8203;21396](https://redirect.github.com/astral-sh/ruff/pull/21396))

##### Rule changes

- \[`refurb`] Detect empty f-strings (`FURB105`) ([#&#8203;21348](https://redirect.github.com/astral-sh/ruff/pull/21348))

##### CLI

- Add option to provide a reason to `--add-noqa` ([#&#8203;21294](https://redirect.github.com/astral-sh/ruff/pull/21294))
- Add upstream linter URL to `ruff linter --output-format=json` ([#&#8203;21316](https://redirect.github.com/astral-sh/ruff/pull/21316))
- Add color to `--help` ([#&#8203;21337](https://redirect.github.com/astral-sh/ruff/pull/21337))

##### Documentation

- Add a new "Opening a PR" section to the contribution guide ([#&#8203;21298](https://redirect.github.com/astral-sh/ruff/pull/21298))
- Added the PyScripter IDE to the list of "Who is using Ruff?" ([#&#8203;21402](https://redirect.github.com/astral-sh/ruff/pull/21402))
- Update PyCharm setup instructions ([#&#8203;21409](https://redirect.github.com/astral-sh/ruff/pull/21409))
- \[`flake8-annotations`] Add link to `allow-star-arg-any` option (`ANN401`) ([#&#8203;21326](https://redirect.github.com/astral-sh/ruff/pull/21326))

##### Other changes

- \[`configuration`] Improve error message when `line-length` exceeds `u16::MAX` ([#&#8203;21329](https://redirect.github.com/astral-sh/ruff/pull/21329))

##### Contributors

- [@&#8203;njhearp](https://redirect.github.com/njhearp)
- [@&#8203;11happy](https://redirect.github.com/11happy)
- [@&#8203;hugovk](https://redirect.github.com/hugovk)
- [@&#8203;Gankra](https://redirect.github.com/Gankra)
- [@&#8203;ntBre](https://redirect.github.com/ntBre)
- [@&#8203;pyscripter](https://redirect.github.com/pyscripter)
- [@&#8203;danparizher](https://redirect.github.com/danparizher)
- [@&#8203;MichaReiser](https://redirect.github.com/MichaReiser)
- [@&#8203;henryiii](https://redirect.github.com/henryiii)
- [@&#8203;charliecloudberry](https://redirect.github.com/charliecloudberry)

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS4xNzMuMSIsInVwZGF0ZWRJblZlciI6IjQxLjE3My4xIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-11-17 01:58_

---

_Merged by @dhruvmanila on 2025-11-17 05:23_

---

_Closed by @dhruvmanila on 2025-11-17 05:23_

---

_Branch deleted on 2025-11-17 05:23_

---
