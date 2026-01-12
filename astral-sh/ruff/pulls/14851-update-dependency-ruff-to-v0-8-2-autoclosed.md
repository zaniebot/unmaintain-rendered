```yaml
number: 14851
title: Update dependency ruff to v0.8.2 - autoclosed
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2024-12-09T00:07:56Z
updated_at: 2024-12-09T00:47:14Z
url: https://github.com/astral-sh/ruff/pull/14851
synced_at: 2026-01-12T15:55:49Z
```

# Update dependency ruff to v0.8.2 - autoclosed

---

_@renovate_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.8.1` -> `==0.8.2` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.8.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.8.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.8.1/0.8.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.8.1/0.8.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.8.2`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#082)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.8.1...0.8.2)

##### Preview features

-   \[`airflow`] Avoid deprecated values (`AIR302`) ([#&#8203;14582](https://redirect.github.com/astral-sh/ruff/pull/14582))
-   \[`airflow`] Extend removed names for `AIR302` ([#&#8203;14734](https://redirect.github.com/astral-sh/ruff/pull/14734))
-   \[`ruff`] Extend `unnecessary-regular-expression` to non-literal strings (`RUF055`) ([#&#8203;14679](https://redirect.github.com/astral-sh/ruff/pull/14679))
-   \[`ruff`] Implement `used-dummy-variable` (`RUF052`) ([#&#8203;14611](https://redirect.github.com/astral-sh/ruff/pull/14611))
-   \[`ruff`] Implement `unnecessary-cast-to-int` (`RUF046`) ([#&#8203;14697](https://redirect.github.com/astral-sh/ruff/pull/14697))

##### Rule changes

-   \[`airflow`] Check `AIR001` from builtin or providers `operators` module ([#&#8203;14631](https://redirect.github.com/astral-sh/ruff/pull/14631))
-   \[`flake8-pytest-style`] Remove `@` in `pytest.mark.parametrize` rule messages ([#&#8203;14770](https://redirect.github.com/astral-sh/ruff/pull/14770))
-   \[`pandas-vet`] Skip rules if the `panda` module hasn't been seen ([#&#8203;14671](https://redirect.github.com/astral-sh/ruff/pull/14671))
-   \[`pylint`] Fix false negatives for `ascii` and `sorted` in `len-as-condition` (`PLC1802`) ([#&#8203;14692](https://redirect.github.com/astral-sh/ruff/pull/14692))
-   \[`refurb`] Guard `hashlib` imports and mark `hashlib-digest-hex` fix as safe (`FURB181`) ([#&#8203;14694](https://redirect.github.com/astral-sh/ruff/pull/14694))

##### Configuration

-   \[`flake8-import-conventions`] Improve syntax check for aliases supplied in configuration for `unconventional-import-alias` (`ICN001`) ([#&#8203;14745](https://redirect.github.com/astral-sh/ruff/pull/14745))

##### Bug fixes

-   Revert: \[pyflakes] Avoid false positives in `@no_type_check` contexts (`F821`, `F722`) ([#&#8203;14615](https://redirect.github.com/astral-sh/ruff/issues/14615)) ([#&#8203;14726](https://redirect.github.com/astral-sh/ruff/pull/14726))
-   \[`pep8-naming`] Avoid false positive for `class Bar(type(foo))` (`N804`) ([#&#8203;14683](https://redirect.github.com/astral-sh/ruff/pull/14683))
-   \[`pycodestyle`] Handle f-strings properly for `invalid-escape-sequence` (`W605`) ([#&#8203;14748](https://redirect.github.com/astral-sh/ruff/pull/14748))
-   \[`pylint`] Ignore `@overload` in `PLR0904` ([#&#8203;14730](https://redirect.github.com/astral-sh/ruff/pull/14730))
-   \[`refurb`] Handle non-finite decimals in `verbose-decimal-constructor` (`FURB157`) ([#&#8203;14596](https://redirect.github.com/astral-sh/ruff/pull/14596))
-   \[`ruff`] Avoid emitting `assignment-in-assert` when all references to the assigned variable are themselves inside `assert`s (`RUF018`) ([#&#8203;14661](https://redirect.github.com/astral-sh/ruff/pull/14661))

##### Documentation

-   Improve docs for `flake8-use-pathlib` rules ([#&#8203;14741](https://redirect.github.com/astral-sh/ruff/pull/14741))
-   Improve error messages and docs for `flake8-comprehensions` rules ([#&#8203;14729](https://redirect.github.com/astral-sh/ruff/pull/14729))
-   \[`flake8-type-checking`] Expands `TC006` docs to better explain itself ([#&#8203;14749](https://redirect.github.com/astral-sh/ruff/pull/14749))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS40Mi40IiwidXBkYXRlZEluVmVyIjoiMzkuNDIuNCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2024-12-09 00:07_

---

_Merged by @AlexWaygood on 2024-12-09 00:46_

---

_Closed by @AlexWaygood on 2024-12-09 00:46_

---

_Branch deleted on 2024-12-09 00:46_

---

_Renamed from "Update dependency ruff to v0.8.2" to "Update dependency ruff to v0.8.2 - autoclosed" by @renovate[bot] on 2024-12-09 00:47_

---
