```yaml
number: 15897
title: Update dependency ruff to v0.9.4
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-02-03T00:25:28Z
updated_at: 2025-02-03T00:31:04Z
url: https://github.com/astral-sh/ruff/pull/15897
synced_at: 2026-01-10T19:57:22Z
```

# Update dependency ruff to v0.9.4

---

_Pull request opened by @renovate on 2025-02-03 00:25_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.9.3` -> `==0.9.4` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.9.4?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.9.4?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.9.3/0.9.4?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.9.3/0.9.4?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.9.4`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#094)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.9.3...0.9.4)

##### Preview features

-   \[`airflow`] Extend airflow context parameter check for `BaseOperator.execute` (`AIR302`) ([#&#8203;15713](https://redirect.github.com/astral-sh/ruff/pull/15713))
-   \[`airflow`] Update `AIR302` to check for deprecated context keys ([#&#8203;15144](https://redirect.github.com/astral-sh/ruff/pull/15144))
-   \[`flake8-bandit`] Permit suspicious imports within stub files (`S4`) ([#&#8203;15822](https://redirect.github.com/astral-sh/ruff/pull/15822))
-   \[`pylint`] Do not trigger `PLR6201` on empty collections ([#&#8203;15732](https://redirect.github.com/astral-sh/ruff/pull/15732))
-   \[`refurb`] Do not emit diagnostic when loop variables are used outside loop body (`FURB122`) ([#&#8203;15757](https://redirect.github.com/astral-sh/ruff/pull/15757))
-   \[`ruff`] Add support for more `re` patterns (`RUF055`) ([#&#8203;15764](https://redirect.github.com/astral-sh/ruff/pull/15764))
-   \[`ruff`] Check for shadowed `map` before suggesting fix (`RUF058`) ([#&#8203;15790](https://redirect.github.com/astral-sh/ruff/pull/15790))
-   \[`ruff`] Do not emit diagnostic when all arguments to `zip()` are variadic (`RUF058`) ([#&#8203;15744](https://redirect.github.com/astral-sh/ruff/pull/15744))
-   \[`ruff`] Parenthesize fix when argument spans multiple lines for `unnecessary-round` (`RUF057`) ([#&#8203;15703](https://redirect.github.com/astral-sh/ruff/pull/15703))

##### Rule changes

-   Preserve quote style in generated code ([#&#8203;15726](https://redirect.github.com/astral-sh/ruff/pull/15726), [#&#8203;15778](https://redirect.github.com/astral-sh/ruff/pull/15778), [#&#8203;15794](https://redirect.github.com/astral-sh/ruff/pull/15794))
-   \[`flake8-bugbear`] Exempt `NewType` calls where the original type is immutable (`B008`) ([#&#8203;15765](https://redirect.github.com/astral-sh/ruff/pull/15765))
-   \[`pylint`] Honor banned top-level imports by `TID253` in `PLC0415`. ([#&#8203;15628](https://redirect.github.com/astral-sh/ruff/pull/15628))
-   \[`pyupgrade`] Ignore `is_typeddict` and `TypedDict` for `deprecated-import` (`UP035`) ([#&#8203;15800](https://redirect.github.com/astral-sh/ruff/pull/15800))

##### CLI

-   Fix formatter warning message for `flake8-quotes` option ([#&#8203;15788](https://redirect.github.com/astral-sh/ruff/pull/15788))
-   Implement tab autocomplete for `ruff config` ([#&#8203;15603](https://redirect.github.com/astral-sh/ruff/pull/15603))

##### Bug fixes

-   \[`flake8-comprehensions`] Do not emit `unnecessary-map` diagnostic when lambda has different arity (`C417`) ([#&#8203;15802](https://redirect.github.com/astral-sh/ruff/pull/15802))
-   \[`flake8-comprehensions`] Parenthesize `sorted` when needed for `unnecessary-call-around-sorted` (`C413`) ([#&#8203;15825](https://redirect.github.com/astral-sh/ruff/pull/15825))
-   \[`pyupgrade`] Handle end-of-line comments for `quoted-annotation` (`UP037`) ([#&#8203;15824](https://redirect.github.com/astral-sh/ruff/pull/15824))

##### Documentation

-   Add missing config docstrings ([#&#8203;15803](https://redirect.github.com/astral-sh/ruff/pull/15803))
-   Add references to `trio.run_process` and `anyio.run_process` ([#&#8203;15761](https://redirect.github.com/astral-sh/ruff/pull/15761))
-   Use `uv init --lib` in tutorial ([#&#8203;15718](https://redirect.github.com/astral-sh/ruff/pull/15718))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xNDUuMCIsInVwZGF0ZWRJblZlciI6IjM5LjE0NS4wIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-02-03 00:25_

---

_Merged by @AlexWaygood on 2025-02-03 00:31_

---

_Closed by @AlexWaygood on 2025-02-03 00:31_

---

_Branch deleted on 2025-02-03 00:31_

---
