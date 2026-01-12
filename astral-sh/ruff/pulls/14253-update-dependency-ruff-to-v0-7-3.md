```yaml
number: 14253
title: Update dependency ruff to v0.7.3
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2024-11-11T00:21:17Z
updated_at: 2024-11-11T08:17:24Z
url: https://github.com/astral-sh/ruff/pull/14253
synced_at: 2026-01-12T15:55:47Z
```

# Update dependency ruff to v0.7.3

---

_@renovate_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.7.2` -> `==0.7.3` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.7.3?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.7.3?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.7.2/0.7.3?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.7.2/0.7.3?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.7.3`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#073)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.7.2...0.7.3)

##### Preview features

-   Formatter: Disallow single-line implicit concatenated strings ([#&#8203;13928](https://redirect.github.com/astral-sh/ruff/pull/13928))
-   \[`flake8-pyi`] Include all Python file types for `PYI006` and `PYI066` ([#&#8203;14059](https://redirect.github.com/astral-sh/ruff/pull/14059))
-   \[`flake8-simplify`] Implement `split-of-static-string` (`SIM905`) ([#&#8203;14008](https://redirect.github.com/astral-sh/ruff/pull/14008))
-   \[`refurb`] Implement `subclass-builtin` (`FURB189`) ([#&#8203;14105](https://redirect.github.com/astral-sh/ruff/pull/14105))
-   \[`ruff`] Improve diagnostic messages and docs (`RUF031`, `RUF032`, `RUF034`) ([#&#8203;14068](https://redirect.github.com/astral-sh/ruff/pull/14068))

##### Rule changes

-   Detect items that hash to same value in duplicate sets (`B033`, `PLC0208`) ([#&#8203;14064](https://redirect.github.com/astral-sh/ruff/pull/14064))
-   \[`eradicate`] Better detection of IntelliJ language injection comments (`ERA001`) ([#&#8203;14094](https://redirect.github.com/astral-sh/ruff/pull/14094))
-   \[`flake8-pyi`] Add autofix for `docstring-in-stub` (`PYI021`) ([#&#8203;14150](https://redirect.github.com/astral-sh/ruff/pull/14150))
-   \[`flake8-pyi`] Update `duplicate-literal-member` (`PYI062`) to alawys provide an autofix ([#&#8203;14188](https://redirect.github.com/astral-sh/ruff/pull/14188))
-   \[`pyflakes`] Detect items that hash to same value in duplicate dictionaries (`F601`) ([#&#8203;14065](https://redirect.github.com/astral-sh/ruff/pull/14065))
-   \[`ruff`] Fix false positive for decorators (`RUF028`) ([#&#8203;14061](https://redirect.github.com/astral-sh/ruff/pull/14061))

##### Bug fixes

-   Avoid parsing joint rule codes as distinct codes in `# noqa` ([#&#8203;12809](https://redirect.github.com/astral-sh/ruff/pull/12809))
-   \[`eradicate`] ignore `# language=` in commented-out-code rule (ERA001) ([#&#8203;14069](https://redirect.github.com/astral-sh/ruff/pull/14069))
-   \[`flake8-bugbear`] - do not run `mutable-argument-default` on stubs (`B006`) ([#&#8203;14058](https://redirect.github.com/astral-sh/ruff/pull/14058))
-   \[`flake8-builtins`] Skip lambda expressions in `builtin-argument-shadowing (A002)` ([#&#8203;14144](https://redirect.github.com/astral-sh/ruff/pull/14144))
-   \[`flake8-comprehension`] Also remove trailing comma while fixing `C409` and `C419` ([#&#8203;14097](https://redirect.github.com/astral-sh/ruff/pull/14097))
-   \[`flake8-simplify`] Allow `open` without context manager in `return` statement (`SIM115`) ([#&#8203;14066](https://redirect.github.com/astral-sh/ruff/pull/14066))
-   \[`pylint`] Respect hash-equivalent literals in `iteration-over-set` (`PLC0208`) ([#&#8203;14063](https://redirect.github.com/astral-sh/ruff/pull/14063))
-   \[`pylint`] Update known dunder methods for Python 3.13 (`PLW3201`) ([#&#8203;14146](https://redirect.github.com/astral-sh/ruff/pull/14146))
-   \[`pyupgrade`] - ignore kwarg unpacking for `UP044` ([#&#8203;14053](https://redirect.github.com/astral-sh/ruff/pull/14053))
-   \[`refurb`] Parse more exotic decimal strings in `verbose-decimal-constructor` (`FURB157`) ([#&#8203;14098](https://redirect.github.com/astral-sh/ruff/pull/14098))

##### Documentation

-   Add links to missing related options within rule documentations ([#&#8203;13971](https://redirect.github.com/astral-sh/ruff/pull/13971))
-   Add rule short code to mkdocs tags to allow searching via rule codes ([#&#8203;14040](https://redirect.github.com/astral-sh/ruff/pull/14040))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS43LjEiLCJ1cGRhdGVkSW5WZXIiOiIzOS43LjEiLCJ0YXJnZXRCcmFuY2giOiJtYWluIiwibGFiZWxzIjpbImludGVybmFsIl19-->


---

_Label `internal` added by @renovate[bot] on 2024-11-11 00:21_

---

_Merged by @AlexWaygood on 2024-11-11 08:17_

---

_Closed by @AlexWaygood on 2024-11-11 08:17_

---

_Branch deleted on 2024-11-11 08:17_

---
