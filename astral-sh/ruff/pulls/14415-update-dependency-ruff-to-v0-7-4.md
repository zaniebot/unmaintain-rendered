```yaml
number: 14415
title: Update dependency ruff to v0.7.4
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2024-11-18T01:28:15Z
updated_at: 2024-11-18T01:41:42Z
url: https://github.com/astral-sh/ruff/pull/14415
synced_at: 2026-01-10T20:50:57Z
```

# Update dependency ruff to v0.7.4

---

_Pull request opened by @renovate on 2024-11-18 01:28_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.7.3` -> `==0.7.4` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.7.4?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.7.4?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.7.3/0.7.4?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.7.3/0.7.4?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.7.4`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#074)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.7.3...0.7.4)

##### Preview features

-   \[`flake8-datetimez`] Detect usages of `datetime.max`/`datetime.min` (`DTZ901`) ([#&#8203;14288](https://redirect.github.com/astral-sh/ruff/pull/14288))
-   \[`flake8-logging`] Implement `root-logger-calls` (`LOG015`) ([#&#8203;14302](https://redirect.github.com/astral-sh/ruff/pull/14302))
-   \[`flake8-no-pep420`] Detect empty implicit namespace packages (`INP001`) ([#&#8203;14236](https://redirect.github.com/astral-sh/ruff/pull/14236))
-   \[`flake8-pyi`] Add "replace with `Self`" fix (`PYI019`) ([#&#8203;14238](https://redirect.github.com/astral-sh/ruff/pull/14238))
-   \[`perflint`] Implement quick-fix for `manual-list-comprehension` (`PERF401`) ([#&#8203;13919](https://redirect.github.com/astral-sh/ruff/pull/13919))
-   \[`pylint`] Implement `shallow-copy-environ` (`W1507`) ([#&#8203;14241](https://redirect.github.com/astral-sh/ruff/pull/14241))
-   \[`ruff`] Implement `none-not-at-end-of-union` (`RUF036`) ([#&#8203;14314](https://redirect.github.com/astral-sh/ruff/pull/14314))
-   \[`ruff`] Implementation `unsafe-markup-call` from `flake8-markupsafe` plugin (`RUF035`) ([#&#8203;14224](https://redirect.github.com/astral-sh/ruff/pull/14224))
-   \[`ruff`] Report problems for `attrs` dataclasses (`RUF008`, `RUF009`) ([#&#8203;14327](https://redirect.github.com/astral-sh/ruff/pull/14327))

##### Rule changes

-   \[`flake8-boolean-trap`] Exclude dunder methods that define operators (`FBT001`) ([#&#8203;14203](https://redirect.github.com/astral-sh/ruff/pull/14203))
-   \[`flake8-pyi`] Add "replace with `Self`" fix (`PYI034`) ([#&#8203;14217](https://redirect.github.com/astral-sh/ruff/pull/14217))
-   \[`flake8-pyi`] Always autofix `duplicate-union-members` (`PYI016`) ([#&#8203;14270](https://redirect.github.com/astral-sh/ruff/pull/14270))
-   \[`flake8-pyi`] Improve autofix for nested and mixed type unions for `unnecessary-type-union` (`PYI055`) ([#&#8203;14272](https://redirect.github.com/astral-sh/ruff/pull/14272))
-   \[`flake8-pyi`] Mark fix as unsafe when type annotation contains comments for `duplicate-literal-member` (`PYI062`) ([#&#8203;14268](https://redirect.github.com/astral-sh/ruff/pull/14268))

##### Server

-   Use the current working directory to resolve settings from `ruff.configuration` ([#&#8203;14352](https://redirect.github.com/astral-sh/ruff/pull/14352))

##### Bug fixes

-   Avoid conflicts between `PLC014` (`useless-import-alias`) and `I002` (`missing-required-import`) by considering `lint.isort.required-imports` for `PLC014` ([#&#8203;14287](https://redirect.github.com/astral-sh/ruff/pull/14287))
-   \[`flake8-type-checking`] Skip quoting annotation if it becomes invalid syntax (`TCH001`)
-   \[`flake8-pyi`] Avoid using `typing.Self` in stub files pre-Python 3.11 (`PYI034`) ([#&#8203;14230](https://redirect.github.com/astral-sh/ruff/pull/14230))
-   \[`flake8-pytest-style`] Flag `pytest.raises` call with keyword argument `expected_exception` (`PT011`) ([#&#8203;14298](https://redirect.github.com/astral-sh/ruff/pull/14298))
-   \[`flake8-simplify`] Infer "unknown" truthiness for literal iterables whose items are all unpacks (`SIM222`) ([#&#8203;14263](https://redirect.github.com/astral-sh/ruff/pull/14263))
-   \[`flake8-type-checking`] Fix false positives for `typing.Annotated` (`TCH001`) ([#&#8203;14311](https://redirect.github.com/astral-sh/ruff/pull/14311))
-   \[`pylint`] Allow `await` at the top-level scope of a notebook (`PLE1142`) ([#&#8203;14225](https://redirect.github.com/astral-sh/ruff/pull/14225))
-   \[`pylint`] Fix miscellaneous issues in `await-outside-async` detection (`PLE1142`) ([#&#8203;14218](https://redirect.github.com/astral-sh/ruff/pull/14218))
-   \[`pyupgrade`] Avoid applying PEP 646 rewrites in invalid contexts (`UP044`) ([#&#8203;14234](https://redirect.github.com/astral-sh/ruff/pull/14234))
-   \[`pyupgrade`] Detect permutations in redundant open modes (`UP015`) ([#&#8203;14255](https://redirect.github.com/astral-sh/ruff/pull/14255))
-   \[`refurb`] Avoid triggering `hardcoded-string-charset` for reordered sets (`FURB156`) ([#&#8203;14233](https://redirect.github.com/astral-sh/ruff/pull/14233))
-   \[`refurb`] Further special cases added to `verbose-decimal-constructor` (`FURB157`) ([#&#8203;14216](https://redirect.github.com/astral-sh/ruff/pull/14216))
-   \[`refurb`] Use `UserString` instead of non-existent `UserStr` (`FURB189`) ([#&#8203;14209](https://redirect.github.com/astral-sh/ruff/pull/14209))
-   \[`ruff`] Avoid treating lowercase letters as `# noqa` codes (`RUF100`) ([#&#8203;14229](https://redirect.github.com/astral-sh/ruff/pull/14229))
-   \[`ruff`] Do not report when `Optional` has no type arguments (`RUF013`) ([#&#8203;14181](https://redirect.github.com/astral-sh/ruff/pull/14181))

##### Documentation

-   Add "Notebook behavior" section for `F704`, `PLE1142` ([#&#8203;14266](https://redirect.github.com/astral-sh/ruff/pull/14266))
-   Document comment policy around fix safety ([#&#8203;14300](https://redirect.github.com/astral-sh/ruff/pull/14300))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xOS4wIiwidXBkYXRlZEluVmVyIjoiMzkuMTkuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2024-11-18 01:28_

---

_Merged by @charliermarsh on 2024-11-18 01:41_

---

_Closed by @charliermarsh on 2024-11-18 01:41_

---

_Branch deleted on 2024-11-18 01:41_

---
