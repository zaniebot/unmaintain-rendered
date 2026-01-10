```yaml
number: 17073
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
created_at: 2025-03-31T01:53:34Z
updated_at: 2025-03-31T07:47:51Z
url: https://github.com/astral-sh/ruff/pull/17073
synced_at: 2026-01-10T19:40:36Z
```

# Update pre-commit dependencies

---

_Pull request opened by @renovate on 2025-03-31 01:53_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [abravalheri/validate-pyproject](https://redirect.github.com/abravalheri/validate-pyproject) | repository | patch | `v0.24` -> `v0.24.1` |
| [astral-sh/ruff-pre-commit](https://redirect.github.com/astral-sh/ruff-pre-commit) | repository | patch | `v0.11.0` -> `v0.11.2` |
| [crate-ci/typos](https://redirect.github.com/crate-ci/typos) | repository | minor | `v1.30.2` -> `v1.31.0` |
| [python-jsonschema/check-jsonschema](https://redirect.github.com/python-jsonschema/check-jsonschema) | repository | minor | `0.31.3` -> `0.32.1` |
| [woodruffw/zizmor-pre-commit](https://redirect.github.com/woodruffw/zizmor-pre-commit) | repository | patch | `v1.5.1` -> `v1.5.2` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

Note: The `pre-commit` manager in Renovate is not supported by the `pre-commit` maintainers or community. Please do not report any problems there, instead [create a Discussion in the Renovate repository](https://redirect.github.com/renovatebot/renovate/discussions/new) if you have any questions.

---

### Release Notes

<details>
<summary>abravalheri/validate-pyproject (abravalheri/validate-pyproject)</summary>

### [`v0.24.1`](https://redirect.github.com/abravalheri/validate-pyproject/releases/tag/v0.24.1)

[Compare Source](https://redirect.github.com/abravalheri/validate-pyproject/compare/v0.24...v0.24.1)

#### What's Changed

-   Fixed multi plugin id was read from the wrong place by [@&#8203;henryiii](https://redirect.github.com/henryiii) in [https://github.com/abravalheri/validate-pyproject/pull/240](https://redirect.github.com/abravalheri/validate-pyproject/pull/240)
-   Implemented alternative plugin sorting, [https://github.com/abravalheri/validate-pyproject/pull/243](https://redirect.github.com/abravalheri/validate-pyproject/pull/243)

**Full Changelog**: https://github.com/abravalheri/validate-pyproject/compare/v0.24...v0.24.1

</details>

<details>
<summary>astral-sh/ruff-pre-commit (astral-sh/ruff-pre-commit)</summary>

### [`v0.11.2`](https://redirect.github.com/astral-sh/ruff-pre-commit/releases/tag/v0.11.2)

[Compare Source](https://redirect.github.com/astral-sh/ruff-pre-commit/compare/v0.11.1...v0.11.2)

See: https://github.com/astral-sh/ruff/releases/tag/0.11.2

### [`v0.11.1`](https://redirect.github.com/astral-sh/ruff-pre-commit/releases/tag/v0.11.1)

[Compare Source](https://redirect.github.com/astral-sh/ruff-pre-commit/compare/v0.11.0...v0.11.1)

See: https://github.com/astral-sh/ruff/releases/tag/0.11.1

</details>

<details>
<summary>crate-ci/typos (crate-ci/typos)</summary>

### [`v1.31.0`](https://redirect.github.com/crate-ci/typos/releases/tag/v1.31.0)

[Compare Source](https://redirect.github.com/crate-ci/typos/compare/v1.30.3...v1.31.0)

#### \[1.31.0] - 2025-03-28

##### Features

-   Updated the dictionary with the [March 2025](https://redirect.github.com/crate-ci/typos/issues/1266) changes

### [`v1.30.3`](https://redirect.github.com/crate-ci/typos/releases/tag/v1.30.3)

[Compare Source](https://redirect.github.com/crate-ci/typos/compare/v1.30.2...v1.30.3)

#### \[1.30.3] - 2025-03-24

##### Features

-   Support detecting `go.work` and `go.work.sum` files

</details>

<details>
<summary>python-jsonschema/check-jsonschema (python-jsonschema/check-jsonschema)</summary>

### [`v0.32.1`](https://redirect.github.com/python-jsonschema/check-jsonschema/blob/HEAD/CHANGELOG.rst#0321)

[Compare Source](https://redirect.github.com/python-jsonschema/check-jsonschema/compare/0.32.0...0.32.1)

-   Fix the `check-meltano` hook to use `types_or`. Thanks
    :user:`edgarrmondragon`! (:pr:`543`)

### [`v0.32.0`](https://redirect.github.com/python-jsonschema/check-jsonschema/blob/HEAD/CHANGELOG.rst#0320)

[Compare Source](https://redirect.github.com/python-jsonschema/check-jsonschema/compare/0.31.3...0.32.0)

-   Update vendored schemas: circle-ci, compose-spec, dependabot, github-workflows,
    gitlab-ci, mergify, renovate, taskfile (2025-03-25)
-   Add Meltano schema and pre-commit hook. Thanks :user:`edgarrmondragon`! (:issue:`540`)
-   Add Snapcraft schema and pre-commit hook. Thanks :user:`fabolhak`! (:issue:`535`)

</details>

<details>
<summary>woodruffw/zizmor-pre-commit (woodruffw/zizmor-pre-commit)</summary>

### [`v1.5.2`](https://redirect.github.com/woodruffw/zizmor-pre-commit/releases/tag/v1.5.2)

[Compare Source](https://redirect.github.com/woodruffw/zizmor-pre-commit/compare/v1.5.1...v1.5.2)

See: https://github.com/woodruffw/zizmor/releases/tag/v1.5.2

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4yMDcuMSIsInVwZGF0ZWRJblZlciI6IjM5LjIwNy4xIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-03-31 01:53_

---

_Review requested from @carljm by @MichaReiser on 2025-03-31 07:28_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-03-31 07:28_

---

_Review requested from @sharkdp by @MichaReiser on 2025-03-31 07:28_

---

_Review requested from @dcreager by @MichaReiser on 2025-03-31 07:28_

---

_Review requested from @MichaReiser by @MichaReiser on 2025-03-31 07:28_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-03-31 07:28_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-03-31 07:28_

---

_Comment by @github-actions[bot] on 2025-03-31 07:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @MichaReiser on 2025-03-31 07:42_

---

_Closed by @MichaReiser on 2025-03-31 07:42_

---

_Branch deleted on 2025-03-31 07:42_

---

_Comment by @github-actions[bot] on 2025-03-31 07:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
