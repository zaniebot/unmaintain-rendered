```yaml
number: 15289
title: Update pre-commit dependencies
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/pre-commit-dependencies
created_at: 2025-01-06T00:55:27Z
updated_at: 2025-01-06T11:35:03Z
url: https://github.com/astral-sh/ruff/pull/15289
synced_at: 2026-01-12T15:55:50Z
```

# Update pre-commit dependencies

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [astral-sh/ruff-pre-commit](https://redirect.github.com/astral-sh/ruff-pre-commit) | repository | patch | `v0.8.4` -> `v0.8.6` |
| [crate-ci/typos](https://redirect.github.com/crate-ci/typos) | repository | minor | `v1.28.4` -> `v1.29.4` |
| [rhysd/actionlint](https://redirect.github.com/rhysd/actionlint) | repository | patch | `v1.7.5` -> `v1.7.6` |
| [woodruffw/zizmor-pre-commit](https://redirect.github.com/woodruffw/zizmor-pre-commit) | repository | major | `v0.10.0` -> `v1.0.0` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

Note: The `pre-commit` manager in Renovate is not supported by the `pre-commit` maintainers or community. Please do not report any problems there, instead [create a Discussion in the Renovate repository](https://redirect.github.com/renovatebot/renovate/discussions/new) if you have any questions.

---

### Release Notes

<details>
<summary>astral-sh/ruff-pre-commit (astral-sh/ruff-pre-commit)</summary>

### [`v0.8.6`](https://redirect.github.com/astral-sh/ruff-pre-commit/releases/tag/v0.8.6)

[Compare Source](https://redirect.github.com/astral-sh/ruff-pre-commit/compare/v0.8.5...v0.8.6)

See: https://github.com/astral-sh/ruff/releases/tag/0.8.6

### [`v0.8.5`](https://redirect.github.com/astral-sh/ruff-pre-commit/releases/tag/v0.8.5)

[Compare Source](https://redirect.github.com/astral-sh/ruff-pre-commit/compare/v0.8.4...v0.8.5)

See: https://github.com/astral-sh/ruff/releases/tag/0.8.5

</details>

<details>
<summary>crate-ci/typos (crate-ci/typos)</summary>

### [`v1.29.4`](https://redirect.github.com/crate-ci/typos/releases/tag/v1.29.4)

[Compare Source](https://redirect.github.com/crate-ci/typos/compare/v1.29.3...v1.29.4)

#### \[1.29.4] - 2025-01-03

### [`v1.29.3`](https://redirect.github.com/crate-ci/typos/releases/tag/v1.29.3)

[Compare Source](https://redirect.github.com/crate-ci/typos/compare/v1.29.2...v1.29.3)

#### \[1.29.3] - 2025-01-02

### [`v1.29.2`](https://redirect.github.com/crate-ci/typos/compare/v1.29.1...v1.29.2)

[Compare Source](https://redirect.github.com/crate-ci/typos/compare/v1.29.1...v1.29.2)

### [`v1.29.1`](https://redirect.github.com/crate-ci/typos/releases/tag/v1.29.1)

[Compare Source](https://redirect.github.com/crate-ci/typos/compare/v1.29.0...v1.29.1)

#### \[1.29.1] - 2025-01-02

##### Fixes

-   Don't correct `deriver`

### [`v1.29.0`](https://redirect.github.com/crate-ci/typos/releases/tag/v1.29.0)

[Compare Source](https://redirect.github.com/crate-ci/typos/compare/v1.28.4...v1.29.0)

#### \[1.29.0] - 2024-12-31

##### Features

-   Updated the dictionary with the [December 2024](https://redirect.github.com/crate-ci/typos/issues/1156) changes

##### Performance

-   Sped up dictionary lookups

</details>

<details>
<summary>rhysd/actionlint (rhysd/actionlint)</summary>

### [`v1.7.6`](https://redirect.github.com/rhysd/actionlint/blob/HEAD/CHANGELOG.md#v176---2025-01-04)

[Compare Source](https://redirect.github.com/rhysd/actionlint/compare/v1.7.5...v1.7.6)

-   Using contexts at specific workflow keys is incorrectly reported as not allowed. Affected workflow keys are as follows. ([#&#8203;495](https://redirect.github.com/rhysd/actionlint/issues/495), [#&#8203;497](https://redirect.github.com/rhysd/actionlint/issues/497), [#&#8203;498](https://redirect.github.com/rhysd/actionlint/issues/498), [#&#8203;500](https://redirect.github.com/rhysd/actionlint/issues/500))
    -   `jobs.<job_id>.steps.with.args`
    -   `jobs.<job_id>.steps.with.entrypoint`
    -   `jobs.<job_id>.services.<service_id>.env`
-   Update Go dependencies to the latest.

\[Changes]\[v1.7.6]

<a id="v1.7.5"></a>

</details>

<details>
<summary>woodruffw/zizmor-pre-commit (woodruffw/zizmor-pre-commit)</summary>

### [`v1.0.0`](https://redirect.github.com/woodruffw/zizmor-pre-commit/releases/tag/v1.0.0)

[Compare Source](https://redirect.github.com/woodruffw/zizmor-pre-commit/compare/v0.10.0...v1.0.0)

See: https://github.com/woodruffw/zizmor/releases/tag/v1.0.0

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

👻 **Immortal**: This PR will be recreated if closed unmerged. Get [config help](https://redirect.github.com/renovatebot/renovate/discussions) if that's undesired.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS44NS4wIiwidXBkYXRlZEluVmVyIjoiMzkuODUuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-01-06 00:55_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-01-06 04:23_

---

_Comment by @AlexWaygood on 2025-01-06 11:29_

I'll undo the zizmor change for now and tackle it in its own PR

---

_@AlexWaygood reviewed on 2025-01-06 11:30_

---

_Review comment by @AlexWaygood on `.pre-commit-config.yaml`:94 on 2025-01-06 11:30_

```suggestion
    rev: v0.10.0
```

---

_Merged by @AlexWaygood on 2025-01-06 11:35_

---

_Closed by @AlexWaygood on 2025-01-06 11:35_

---

_Branch deleted on 2025-01-06 11:35_

---
