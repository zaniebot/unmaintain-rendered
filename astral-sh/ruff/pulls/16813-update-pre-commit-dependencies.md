```yaml
number: 16813
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
created_at: 2025-03-17T15:31:08Z
updated_at: 2025-03-17T15:37:16Z
url: https://github.com/astral-sh/ruff/pull/16813
synced_at: 2026-01-10T19:49:02Z
```

# Update pre-commit dependencies

---

_Pull request opened by @renovate on 2025-03-17 15:31_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [abravalheri/validate-pyproject](https://redirect.github.com/abravalheri/validate-pyproject) | repository | minor | `v0.23` -> `v0.24` |
| [astral-sh/ruff-pre-commit](https://redirect.github.com/astral-sh/ruff-pre-commit) | repository | minor | `v0.9.9` -> `v0.11.0` |
| [crate-ci/typos](https://redirect.github.com/crate-ci/typos) | repository | patch | `v1.30.0` -> `v1.30.2` |
| [python-jsonschema/check-jsonschema](https://redirect.github.com/python-jsonschema/check-jsonschema) | repository | patch | `0.31.2` -> `0.31.3` |
| [rbubley/mirrors-prettier](https://redirect.github.com/rbubley/mirrors-prettier) | repository | patch | `v3.5.2` -> `v3.5.3` |
| [woodruffw/zizmor-pre-commit](https://redirect.github.com/woodruffw/zizmor-pre-commit) | repository | minor | `v1.4.1` -> `v1.5.1` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

Note: The `pre-commit` manager in Renovate is not supported by the `pre-commit` maintainers or community. Please do not report any problems there, instead [create a Discussion in the Renovate repository](https://redirect.github.com/renovatebot/renovate/discussions/new) if you have any questions.

---

### Release Notes

<details>
<summary>abravalheri/validate-pyproject (abravalheri/validate-pyproject)</summary>

### [`v0.24`](https://redirect.github.com/abravalheri/validate-pyproject/releases/tag/v0.24)

[Compare Source](https://redirect.github.com/abravalheri/validate-pyproject/compare/v0.23...v0.24)

#### What's Changed

-   Fix integration with `SchemaStore` by loading extra/side schemas, [#&#8203;226](https://redirect.github.com/abravalheri/validate-pyproject/issues/226), [#&#8203;229](https://redirect.github.com/abravalheri/validate-pyproject/issues/229).
-   Add support for loading extra schemas, [#&#8203;226](https://redirect.github.com/abravalheri/validate-pyproject/issues/226).
-   Fixed verify author dict is not empty, [#&#8203;232](https://redirect.github.com/abravalheri/validate-pyproject/issues/232).
-   Added support for `validate_pyproject.multi_schema` plugins with extra schemas, [#&#8203;231](https://redirect.github.com/abravalheri/validate-pyproject/issues/231).
-   `validate-pyproject` no longer communicates test dependencies via the `tests` extra and documentation dependencies dependencies via the `docs/requirements.txt` file. Instead [`dependency-groups`](https://packaging.python.org/en/latest/specifications/dependency-groups/) have been adopted to support CI environments, [#&#8203;227](https://redirect.github.com/abravalheri/validate-pyproject/issues/227).

    As a result, `uv`'s high level interface also works for developers. You can use the [`dependency-groups`](https://pypi.org/project/dependency-groups/) package on PyPI if you need to convert to a classic requirements list.

Contributions by [@&#8203;henryiii](https://redirect.github.com/henryiii).

**Full Changelog**: https://github.com/abravalheri/validate-pyproject/compare/v0.23...v0.24

</details>

<details>
<summary>astral-sh/ruff-pre-commit (astral-sh/ruff-pre-commit)</summary>

### [`v0.11.0`](https://redirect.github.com/astral-sh/ruff-pre-commit/releases/tag/v0.11.0)

[Compare Source](https://redirect.github.com/astral-sh/ruff-pre-commit/compare/v0.10.0...v0.11.0)

See: https://github.com/astral-sh/ruff/releases/tag/0.11.0

### [`v0.10.0`](https://redirect.github.com/astral-sh/ruff-pre-commit/releases/tag/v0.10.0)

[Compare Source](https://redirect.github.com/astral-sh/ruff-pre-commit/compare/v0.9.10...v0.10.0)

See: https://github.com/astral-sh/ruff/releases/tag/0.10.0

### [`v0.9.10`](https://redirect.github.com/astral-sh/ruff-pre-commit/releases/tag/v0.9.10)

[Compare Source](https://redirect.github.com/astral-sh/ruff-pre-commit/compare/v0.9.9...v0.9.10)

See: https://github.com/astral-sh/ruff/releases/tag/0.9.10

</details>

<details>
<summary>crate-ci/typos (crate-ci/typos)</summary>

### [`v1.30.2`](https://redirect.github.com/crate-ci/typos/releases/tag/v1.30.2)

[Compare Source](https://redirect.github.com/crate-ci/typos/compare/v1.30.1...v1.30.2)

#### \[1.30.2] - 2025-03-10

##### Features

-   Add `--highlight-words` and `--highlight-identifiers` for easier debugging of config

### [`v1.30.1`](https://redirect.github.com/crate-ci/typos/releases/tag/v1.30.1)

[Compare Source](https://redirect.github.com/crate-ci/typos/compare/v1.30.0...v1.30.1)

#### \[1.30.1] - 2025-03-04

##### Features

-   *(action)* Create `v1` tag

</details>

<details>
<summary>python-jsonschema/check-jsonschema (python-jsonschema/check-jsonschema)</summary>

### [`v0.31.3`](https://redirect.github.com/python-jsonschema/check-jsonschema/blob/HEAD/CHANGELOG.rst#0313)

[Compare Source](https://redirect.github.com/python-jsonschema/check-jsonschema/compare/0.31.2...0.31.3)

-   Update vendored schemas: circle-ci, github-workflows, mergify, renovate, taskfile
    (2025-03-07)

</details>

<details>
<summary>rbubley/mirrors-prettier (rbubley/mirrors-prettier)</summary>

### [`v3.5.3`](https://redirect.github.com/rbubley/mirrors-prettier/compare/v3.5.2...v3.5.3)

[Compare Source](https://redirect.github.com/rbubley/mirrors-prettier/compare/v3.5.2...v3.5.3)

</details>

<details>
<summary>woodruffw/zizmor-pre-commit (woodruffw/zizmor-pre-commit)</summary>

### [`v1.5.1`](https://redirect.github.com/woodruffw/zizmor-pre-commit/releases/tag/v1.5.1)

[Compare Source](https://redirect.github.com/woodruffw/zizmor-pre-commit/compare/v1.5.0...v1.5.1)

See: https://github.com/woodruffw/zizmor/releases/tag/v1.5.1

### [`v1.5.0`](https://redirect.github.com/woodruffw/zizmor-pre-commit/releases/tag/v1.5.0)

[Compare Source](https://redirect.github.com/woodruffw/zizmor-pre-commit/compare/v1.4.1...v1.5.0)

See: https://github.com/woodruffw/zizmor/releases/tag/v1.5.0

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

_Label `internal` added by @renovate[bot] on 2025-03-17 15:31_

---

_Merged by @AlexWaygood on 2025-03-17 15:37_

---

_Closed by @AlexWaygood on 2025-03-17 15:37_

---

_Branch deleted on 2025-03-17 15:37_

---
