```yaml
number: 12827
title: Update dependency black to v24.8.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/black-24.x
created_at: 2024-08-12T02:02:44Z
updated_at: 2024-08-12T04:26:29Z
url: https://github.com/astral-sh/ruff/pull/12827
synced_at: 2026-01-12T15:55:42Z
```

# Update dependency black to v24.8.0

---

_@renovate_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [black](https://togithub.com/psf/black) ([changelog](https://togithub.com/psf/black/blob/main/CHANGES.md)) | `==24.3.0` -> `==24.8.0` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/black/24.8.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/black/24.8.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/black/24.3.0/24.8.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/black/24.3.0/24.8.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

### Release Notes

<details>
<summary>psf/black (black)</summary>

### [`v24.8.0`](https://togithub.com/psf/black/blob/HEAD/CHANGES.md#2480)

[Compare Source](https://togithub.com/psf/black/compare/24.4.2...24.8.0)

##### Stable style

-   Fix crash when `# fmt: off` is used before a closing parenthesis or bracket. ([#&#8203;4363](https://togithub.com/psf/black/issues/4363))

##### Packaging

-   Packaging metadata updated: docs are explictly linked, the issue tracker is now also
    linked. This improves the PyPI listing for Black. ([#&#8203;4345](https://togithub.com/psf/black/issues/4345))

##### Parser

-   Fix regression where Black failed to parse a multiline f-string containing another
    multiline string ([#&#8203;4339](https://togithub.com/psf/black/issues/4339))
-   Fix regression where Black failed to parse an escaped single quote inside an f-string
    ([#&#8203;4401](https://togithub.com/psf/black/issues/4401))
-   Fix bug with Black incorrectly parsing empty lines with a backslash ([#&#8203;4343](https://togithub.com/psf/black/issues/4343))
-   Fix bugs with Black's tokenizer not handling `\{` inside f-strings very well ([#&#8203;4422](https://togithub.com/psf/black/issues/4422))
-   Fix incorrect line numbers in the tokenizer for certain tokens within f-strings
    ([#&#8203;4423](https://togithub.com/psf/black/issues/4423))

##### Performance

-   Improve performance when a large directory is listed in `.gitignore` ([#&#8203;4415](https://togithub.com/psf/black/issues/4415))

##### *Blackd*

-   Fix blackd (and all extras installs) for docker container ([#&#8203;4357](https://togithub.com/psf/black/issues/4357))

### [`v24.4.2`](https://togithub.com/psf/black/blob/HEAD/CHANGES.md#2442)

[Compare Source](https://togithub.com/psf/black/compare/24.4.1...24.4.2)

This is a bugfix release to fix two regressions in the new f-string parser introduced in
24.4.1.

##### Parser

-   Fix regression where certain complex f-strings failed to parse ([#&#8203;4332](https://togithub.com/psf/black/issues/4332))

##### Performance

-   Fix bad performance on certain complex string literals ([#&#8203;4331](https://togithub.com/psf/black/issues/4331))

### [`v24.4.1`](https://togithub.com/psf/black/blob/HEAD/CHANGES.md#2441)

[Compare Source](https://togithub.com/psf/black/compare/24.4.0...24.4.1)

##### Highlights

-   Add support for the new Python 3.12 f-string syntax introduced by PEP 701 ([#&#8203;3822](https://togithub.com/psf/black/issues/3822))

##### Stable style

-   Fix crash involving indented dummy functions containing newlines ([#&#8203;4318](https://togithub.com/psf/black/issues/4318))

##### Parser

-   Add support for type parameter defaults, a new syntactic feature added to Python 3.13
    by PEP 696 ([#&#8203;4327](https://togithub.com/psf/black/issues/4327))

##### Integrations

-   Github Action now works even when `git archive` is skipped ([#&#8203;4313](https://togithub.com/psf/black/issues/4313))

### [`v24.4.0`](https://togithub.com/psf/black/blob/HEAD/CHANGES.md#2440)

[Compare Source](https://togithub.com/psf/black/compare/24.3.0...24.4.0)

##### Stable style

-   Fix unwanted crashes caused by AST equivalency check ([#&#8203;4290](https://togithub.com/psf/black/issues/4290))

##### Preview style

-   `if` guards in `case` blocks are now wrapped in parentheses when the line is too long.
    ([#&#8203;4269](https://togithub.com/psf/black/issues/4269))
-   Stop moving multiline strings to a new line unless inside brackets ([#&#8203;4289](https://togithub.com/psf/black/issues/4289))

##### Integrations

-   Add a new option `use_pyproject` to the GitHub Action `psf/black`. This will read the
    Black version from `pyproject.toml`. ([#&#8203;4294](https://togithub.com/psf/black/issues/4294))

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

This PR was generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOC4yMC4xIiwidXBkYXRlZEluVmVyIjoiMzguMjAuMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2024-08-12 02:02_

---

_Comment by @dhruvmanila on 2024-08-12 04:26_

The [`scripts/checks_docs_formatted.py` didn't fail](https://github.com/astral-sh/ruff/actions/runs/10344735329/job/28630721934?pr=12827#step:11:1), no formatting changes required here. This is good to go.

---

_Merged by @dhruvmanila on 2024-08-12 04:26_

---

_Closed by @dhruvmanila on 2024-08-12 04:26_

---

_Branch deleted on 2024-08-12 04:26_

---
