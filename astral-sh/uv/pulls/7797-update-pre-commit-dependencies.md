```yaml
number: 7797
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
created_at: 2024-09-30T01:02:02Z
updated_at: 2024-09-30T02:52:58Z
url: https://github.com/astral-sh/uv/pull/7797
synced_at: 2026-01-10T12:53:56Z
```

# Update pre-commit dependencies

---

_Pull request opened by @renovate on 2024-09-30 01:02_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [abravalheri/validate-pyproject](https://redirect.github.com/abravalheri/validate-pyproject) | repository | minor | `v0.19` -> `v0.20.2` |
| [astral-sh/ruff-pre-commit](https://redirect.github.com/astral-sh/ruff-pre-commit) | repository | patch | `v0.6.7` -> `v0.6.8` |

Note: The `pre-commit` manager in Renovate is not supported by the `pre-commit` maintainers or community. Please do not report any problems there, instead [create a Discussion in the Renovate repository](https://redirect.github.com/renovatebot/renovate/discussions/new) if you have any questions.

---

### Release Notes

<details>
<summary>abravalheri/validate-pyproject (abravalheri/validate-pyproject)</summary>

### [`v0.20.2`](https://redirect.github.com/abravalheri/validate-pyproject/releases/tag/v0.20.2)

[Compare Source](https://redirect.github.com/abravalheri/validate-pyproject/compare/v0.20.1...v0.20.2)

Attempt to solve problems with release pipeline (CI), related to [https://github.com/actions/upload-artifact/issues/618](https://redirect.github.com/actions/upload-artifact/issues/618).
The package code is unchanged regarding [v0.20](https://redirect.github.com/abravalheri/validate-pyproject/releases/tag/v0.20).

**Full Changelog**: https://github.com/abravalheri/validate-pyproject/compare/v0.20.1...v0.20.2

### [`v0.20.1`](https://redirect.github.com/abravalheri/validate-pyproject/releases/tag/v0.20.1)

[Compare Source](https://redirect.github.com/abravalheri/validate-pyproject/compare/v0.20...v0.20.1)

Attempt to solve problems with release pipeline (CI).
The package code is unchanged regarding [v0.20](https://redirect.github.com/abravalheri/validate-pyproject/releases/tag/v0.20)

**Full Changelog**: https://github.com/abravalheri/validate-pyproject/compare/v0.20...v0.20.1

### [`v0.20`](https://redirect.github.com/abravalheri/validate-pyproject/releases/tag/v0.20)

[Compare Source](https://redirect.github.com/abravalheri/validate-pyproject/compare/v0.19...v0.20)

#### What's Changed

-   `setuptools` plugin:
    -   Update `setuptools.schema.json` in [https://github.com/abravalheri/validate-pyproject/pull/206](https://redirect.github.com/abravalheri/validate-pyproject/pull/206)

#### Maintenance and Minor Changes

-   Fix indentation in readme's pre-commit config by [@&#8203;jvacek](https://redirect.github.com/jvacek) in [https://github.com/abravalheri/validate-pyproject/pull/195](https://redirect.github.com/abravalheri/validate-pyproject/pull/195)
-   Fix misplaced comments on `formats.py` in [https://github.com/abravalheri/validate-pyproject/pull/196](https://redirect.github.com/abravalheri/validate-pyproject/pull/196)
-   Adopt `--import-mode=importlib` for pytest to prevent errors with `importlib.metadata` in [https://github.com/abravalheri/validate-pyproject/pull/203](https://redirect.github.com/abravalheri/validate-pyproject/pull/203)
-   Fix Cirrus macOS by [@&#8203;henryiii](https://redirect.github.com/henryiii) in [https://github.com/abravalheri/validate-pyproject/pull/202](https://redirect.github.com/abravalheri/validate-pyproject/pull/202)
-   Update Cirrus CI config from latest PyScaffold template in [https://github.com/abravalheri/validate-pyproject/pull/204](https://redirect.github.com/abravalheri/validate-pyproject/pull/204)
-   Attempt building and linting in a single task for Cirrus CI in [https://github.com/abravalheri/validate-pyproject/pull/205](https://redirect.github.com/abravalheri/validate-pyproject/pull/205)

#### New Contributors

-   [@&#8203;jvacek](https://redirect.github.com/jvacek) made their first contribution in [https://github.com/abravalheri/validate-pyproject/pull/195](https://redirect.github.com/abravalheri/validate-pyproject/pull/195)

**Full Changelog**: https://github.com/abravalheri/validate-pyproject/compare/v0.19...v0.20

</details>

<details>
<summary>astral-sh/ruff-pre-commit (astral-sh/ruff-pre-commit)</summary>

### [`v0.6.8`](https://redirect.github.com/astral-sh/ruff-pre-commit/releases/tag/v0.6.8)

[Compare Source](https://redirect.github.com/astral-sh/ruff-pre-commit/compare/v0.6.7...v0.6.8)

See: https://github.com/astral-sh/ruff/releases/tag/0.6.8

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

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOC45Ny4wIiwidXBkYXRlZEluVmVyIjoiMzguOTcuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2024-09-30 01:02_

---

_Merged by @charliermarsh on 2024-09-30 02:52_

---

_Closed by @charliermarsh on 2024-09-30 02:52_

---

_Branch deleted on 2024-09-30 02:52_

---
