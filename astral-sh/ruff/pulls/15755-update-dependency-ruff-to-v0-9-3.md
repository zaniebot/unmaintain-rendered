```yaml
number: 15755
title: Update dependency ruff to v0.9.3
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-01-27T01:41:36Z
updated_at: 2025-01-27T03:25:57Z
url: https://github.com/astral-sh/ruff/pull/15755
synced_at: 2026-01-12T15:55:52Z
```

# Update dependency ruff to v0.9.3

---

_@renovate_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.9.2` -> `==0.9.3` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.9.3?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.9.3?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.9.2/0.9.3?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.9.2/0.9.3?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.9.3`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#093)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.9.2...0.9.3)

##### Preview features

-   \[`airflow`] Argument `fail_stop` in DAG has been renamed as `fail_fast` (`AIR302`) ([#&#8203;15633](https://redirect.github.com/astral-sh/ruff/pull/15633))
-   \[`airflow`] Extend `AIR303` with more symbols ([#&#8203;15611](https://redirect.github.com/astral-sh/ruff/pull/15611))
-   \[`flake8-bandit`] Report all references to suspicious functions (`S3`) ([#&#8203;15541](https://redirect.github.com/astral-sh/ruff/pull/15541))
-   \[`flake8-pytest-style`] Do not emit diagnostics for empty `for` loops (`PT012`, `PT031`) ([#&#8203;15542](https://redirect.github.com/astral-sh/ruff/pull/15542))
-   \[`flake8-simplify`] Avoid double negations (`SIM103`) ([#&#8203;15562](https://redirect.github.com/astral-sh/ruff/pull/15562))
-   \[`pyflakes`] Fix infinite loop with unused local import in `__init__.py` (`F401`) ([#&#8203;15517](https://redirect.github.com/astral-sh/ruff/pull/15517))
-   \[`pylint`] Do not report methods with only one `EM101`-compatible `raise` (`PLR6301`) ([#&#8203;15507](https://redirect.github.com/astral-sh/ruff/pull/15507))
-   \[`pylint`] Implement `redefined-slots-in-subclass` (`W0244`) ([#&#8203;9640](https://redirect.github.com/astral-sh/ruff/pull/9640))
-   \[`pyupgrade`] Add rules to use PEP 695 generics in classes and functions (`UP046`, `UP047`) ([#&#8203;15565](https://redirect.github.com/astral-sh/ruff/pull/15565), [#&#8203;15659](https://redirect.github.com/astral-sh/ruff/pull/15659))
-   \[`refurb`] Implement `for-loop-writes` (`FURB122`) ([#&#8203;10630](https://redirect.github.com/astral-sh/ruff/pull/10630))
-   \[`ruff`] Implement `needless-else` clause (`RUF047`) ([#&#8203;15051](https://redirect.github.com/astral-sh/ruff/pull/15051))
-   \[`ruff`] Implement `starmap-zip` (`RUF058`) ([#&#8203;15483](https://redirect.github.com/astral-sh/ruff/pull/15483))

##### Rule changes

-   \[`flake8-bugbear`] Do not raise error if keyword argument is present and target-python version is less or equals than 3.9 (`B903`) ([#&#8203;15549](https://redirect.github.com/astral-sh/ruff/pull/15549))
-   \[`flake8-comprehensions`] strip parentheses around generators in `unnecessary-generator-set` (`C401`) ([#&#8203;15553](https://redirect.github.com/astral-sh/ruff/pull/15553))
-   \[`flake8-pytest-style`] Rewrite references to `.exception` (`PT027`) ([#&#8203;15680](https://redirect.github.com/astral-sh/ruff/pull/15680))
-   \[`flake8-simplify`] Mark fixes as unsafe (`SIM201`, `SIM202`) ([#&#8203;15626](https://redirect.github.com/astral-sh/ruff/pull/15626))
-   \[`flake8-type-checking`] Fix some safe fixes being labeled unsafe (`TC006`,`TC008`) ([#&#8203;15638](https://redirect.github.com/astral-sh/ruff/pull/15638))
-   \[`isort`] Omit trailing whitespace in `unsorted-imports` (`I001`) ([#&#8203;15518](https://redirect.github.com/astral-sh/ruff/pull/15518))
-   \[`pydoclint`] Allow ignoring one line docstrings for `DOC` rules ([#&#8203;13302](https://redirect.github.com/astral-sh/ruff/pull/13302))
-   \[`pyflakes`] Apply redefinition fixes by source code order (`F811`) ([#&#8203;15575](https://redirect.github.com/astral-sh/ruff/pull/15575))
-   \[`pyflakes`] Avoid removing too many imports in `redefined-while-unused` (`F811`) ([#&#8203;15585](https://redirect.github.com/astral-sh/ruff/pull/15585))
-   \[`pyflakes`] Group redefinition fixes by source statement (`F811`) ([#&#8203;15574](https://redirect.github.com/astral-sh/ruff/pull/15574))
-   \[`pylint`] Include name of base class in message for `redefined-slots-in-subclass` (`W0244`) ([#&#8203;15559](https://redirect.github.com/astral-sh/ruff/pull/15559))
-   \[`ruff`] Update fix for `RUF055` to use `var == value` ([#&#8203;15605](https://redirect.github.com/astral-sh/ruff/pull/15605))

##### Formatter

-   Fix bracket spacing for single-element tuples in f-string expressions ([#&#8203;15537](https://redirect.github.com/astral-sh/ruff/pull/15537))
-   Fix unstable f-string formatting for expressions containing a trailing comma ([#&#8203;15545](https://redirect.github.com/astral-sh/ruff/pull/15545))

##### Performance

-   Avoid quadratic membership check in import fixes ([#&#8203;15576](https://redirect.github.com/astral-sh/ruff/pull/15576))

##### Server

-   Allow `unsafe-fixes` settings for code actions ([#&#8203;15666](https://redirect.github.com/astral-sh/ruff/pull/15666))

##### Bug fixes

-   \[`flake8-bandit`] Add missing single-line/dotall regex flag (`S608`) ([#&#8203;15654](https://redirect.github.com/astral-sh/ruff/pull/15654))
-   \[`flake8-import-conventions`] Fix infinite loop between `ICN001` and `I002` (`ICN001`) ([#&#8203;15480](https://redirect.github.com/astral-sh/ruff/pull/15480))
-   \[`flake8-simplify`] Do not emit diagnostics for expressions inside string type annotations (`SIM222`, `SIM223`) ([#&#8203;15405](https://redirect.github.com/astral-sh/ruff/pull/15405))
-   \[`pyflakes`] Treat arguments passed to the `default=` parameter of `TypeVar` as type expressions (`F821`) ([#&#8203;15679](https://redirect.github.com/astral-sh/ruff/pull/15679))
-   \[`pyupgrade`] Avoid syntax error when the iterable is a non-parenthesized tuple (`UP028`) ([#&#8203;15543](https://redirect.github.com/astral-sh/ruff/pull/15543))
-   \[`ruff`] Exempt `NewType` calls where the original type is immutable (`RUF009`) ([#&#8203;15588](https://redirect.github.com/astral-sh/ruff/pull/15588))
-   Preserve raw string prefix and escapes in all codegen fixes ([#&#8203;15694](https://redirect.github.com/astral-sh/ruff/pull/15694))

##### Documentation

-   Generate documentation redirects for lowercase rule codes ([#&#8203;15564](https://redirect.github.com/astral-sh/ruff/pull/15564))
-   `TRY300`: Add some extra notes on not catching exceptions you didn't expect ([#&#8203;15036](https://redirect.github.com/astral-sh/ruff/pull/15036))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xMjUuMSIsInVwZGF0ZWRJblZlciI6IjM5LjEyNS4xIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-01-27 01:41_

---

_Merged by @charliermarsh on 2025-01-27 03:25_

---

_Closed by @charliermarsh on 2025-01-27 03:25_

---

_Branch deleted on 2025-01-27 03:25_

---
