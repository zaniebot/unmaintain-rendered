```yaml
number: 16062
title: Update dependency ruff to v0.9.5
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-02-10T00:25:36Z
updated_at: 2025-02-10T02:29:07Z
url: https://github.com/astral-sh/ruff/pull/16062
synced_at: 2026-01-10T19:57:22Z
```

# Update dependency ruff to v0.9.5

---

_Pull request opened by @renovate on 2025-02-10 00:25_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.9.4` -> `==0.9.5` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.9.5?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.9.5?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.9.4/0.9.5?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.9.4/0.9.5?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.9.5`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#095)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.9.4...0.9.5)

##### Preview features

-   Recognize all symbols named `TYPE_CHECKING` for `in_type_checking_block` ([#&#8203;15719](https://redirect.github.com/astral-sh/ruff/pull/15719))
-   \[`flake8-comprehensions`] Handle builtins at top of file correctly for `unnecessary-dict-comprehension-for-iterable` (`C420`) ([#&#8203;15837](https://redirect.github.com/astral-sh/ruff/pull/15837))
-   \[`flake8-logging`] `.exception()` and `exc_info=` outside exception handlers (`LOG004`, `LOG014`) ([#&#8203;15799](https://redirect.github.com/astral-sh/ruff/pull/15799))
-   \[`flake8-pyi`] Fix incorrect behaviour of `custom-typevar-return-type` preview-mode autofix if `typing` was already imported (`PYI019`) ([#&#8203;15853](https://redirect.github.com/astral-sh/ruff/pull/15853))
-   \[`flake8-pyi`] Fix more complex cases (`PYI019`) ([#&#8203;15821](https://redirect.github.com/astral-sh/ruff/pull/15821))
-   \[`flake8-pyi`] Make `PYI019` autofixable for `.py` files in preview mode as well as stubs ([#&#8203;15889](https://redirect.github.com/astral-sh/ruff/pull/15889))
-   \[`flake8-pyi`] Remove type parameter correctly when it is the last (`PYI019`) ([#&#8203;15854](https://redirect.github.com/astral-sh/ruff/pull/15854))
-   \[`pylint`] Fix missing parens in unsafe fix for `unnecessary-dunder-call` (`PLC2801`) ([#&#8203;15762](https://redirect.github.com/astral-sh/ruff/pull/15762))
-   \[`pyupgrade`] Better messages and diagnostic range (`UP015`) ([#&#8203;15872](https://redirect.github.com/astral-sh/ruff/pull/15872))
-   \[`pyupgrade`] Rename private type parameters in PEP 695 generics (`UP049`) ([#&#8203;15862](https://redirect.github.com/astral-sh/ruff/pull/15862))
-   \[`refurb`] Also report non-name expressions (`FURB169`) ([#&#8203;15905](https://redirect.github.com/astral-sh/ruff/pull/15905))
-   \[`refurb`] Mark fix as unsafe if there are comments (`FURB171`) ([#&#8203;15832](https://redirect.github.com/astral-sh/ruff/pull/15832))
-   \[`ruff`] Classes with mixed type variable style (`RUF053`) ([#&#8203;15841](https://redirect.github.com/astral-sh/ruff/pull/15841))
-   \[`airflow`] `BashOperator` has been moved to `airflow.providers.standard.operators.bash.BashOperator` (`AIR302`) ([#&#8203;15922](https://redirect.github.com/astral-sh/ruff/pull/15922))
-   \[`flake8-pyi`] Add autofix for unused-private-type-var (`PYI018`) ([#&#8203;15999](https://redirect.github.com/astral-sh/ruff/pull/15999))
-   \[`flake8-pyi`] Significantly improve accuracy of `PYI019` if preview mode is enabled ([#&#8203;15888](https://redirect.github.com/astral-sh/ruff/pull/15888))

##### Rule changes

-   Preserve triple quotes and prefixes for strings ([#&#8203;15818](https://redirect.github.com/astral-sh/ruff/pull/15818))
-   \[`flake8-comprehensions`] Skip when `TypeError` present from too many (kw)args for `C410`,`C411`, and `C418` ([#&#8203;15838](https://redirect.github.com/astral-sh/ruff/pull/15838))
-   \[`flake8-pyi`] Rename `PYI019` and improve its diagnostic message ([#&#8203;15885](https://redirect.github.com/astral-sh/ruff/pull/15885))
-   \[`pep8-naming`] Ignore `@override` methods (`N803`) ([#&#8203;15954](https://redirect.github.com/astral-sh/ruff/pull/15954))
-   \[`pyupgrade`] Reuse replacement logic from `UP046` and `UP047` to preserve more comments (`UP040`) ([#&#8203;15840](https://redirect.github.com/astral-sh/ruff/pull/15840))
-   \[`ruff`] Analyze deferred annotations before enforcing `mutable-(data)class-default` and `function-call-in-dataclass-default-argument` (`RUF008`,`RUF009`,`RUF012`) ([#&#8203;15921](https://redirect.github.com/astral-sh/ruff/pull/15921))
-   \[`pycodestyle`] Exempt `sys.path += ...` calls (`E402`) ([#&#8203;15980](https://redirect.github.com/astral-sh/ruff/pull/15980))

##### Configuration

-   Config error only when `flake8-import-conventions` alias conflicts with `isort.required-imports` bound name ([#&#8203;15918](https://redirect.github.com/astral-sh/ruff/pull/15918))
-   Workaround Even Better TOML crash related to `allOf` ([#&#8203;15992](https://redirect.github.com/astral-sh/ruff/pull/15992))

##### Bug fixes

-   \[`flake8-comprehensions`] Unnecessary `list` comprehension (rewrite as a `set` comprehension) (`C403`) - Handle extraneous parentheses around list comprehension ([#&#8203;15877](https://redirect.github.com/astral-sh/ruff/pull/15877))
-   \[`flake8-comprehensions`] Handle trailing comma in fixes for `unnecessary-generator-list/set` (`C400`,`C401`) ([#&#8203;15929](https://redirect.github.com/astral-sh/ruff/pull/15929))
-   \[`flake8-pyi`] Fix several correctness issues with `custom-type-var-return-type` (`PYI019`) ([#&#8203;15851](https://redirect.github.com/astral-sh/ruff/pull/15851))
-   \[`pep8-naming`] Consider any number of leading underscore for `N801` ([#&#8203;15988](https://redirect.github.com/astral-sh/ruff/pull/15988))
-   \[`pyflakes`] Visit forward annotations in `TypeAliasType` as types (`F401`) ([#&#8203;15829](https://redirect.github.com/astral-sh/ruff/pull/15829))
-   \[`pylint`] Correct min/max auto-fix and suggestion for (`PL1730`) ([#&#8203;15930](https://redirect.github.com/astral-sh/ruff/pull/15930))
-   \[`refurb`] Handle unparenthesized tuples correctly (`FURB122`, `FURB142`) ([#&#8203;15953](https://redirect.github.com/astral-sh/ruff/pull/15953))
-   \[`refurb`] Avoid `None | None` as well as better detection and fix (`FURB168`) ([#&#8203;15779](https://redirect.github.com/astral-sh/ruff/pull/15779))

##### Documentation

-   Add deprecation warning for `ruff-lsp` related settings ([#&#8203;15850](https://redirect.github.com/astral-sh/ruff/pull/15850))
-   Docs (`linter.md`): clarify that Python files are always searched for in subdirectories ([#&#8203;15882](https://redirect.github.com/astral-sh/ruff/pull/15882))
-   Fix a typo in `non_pep695_generic_class.rs` ([#&#8203;15946](https://redirect.github.com/astral-sh/ruff/pull/15946))
-   Improve Docs: Pylint subcategories' codes ([#&#8203;15909](https://redirect.github.com/astral-sh/ruff/pull/15909))
-   Remove non-existing `lint.extendIgnore` editor setting ([#&#8203;15844](https://redirect.github.com/astral-sh/ruff/pull/15844))
-   Update black deviations ([#&#8203;15928](https://redirect.github.com/astral-sh/ruff/pull/15928))
-   Mention `UP049` in `UP046` and `UP047`, add `See also` section to `UP040` ([#&#8203;15956](https://redirect.github.com/astral-sh/ruff/pull/15956))
-   Add instance variable examples to `RUF012` ([#&#8203;15982](https://redirect.github.com/astral-sh/ruff/pull/15982))
-   Explain precedence for `ignore` and `select` config ([#&#8203;15883](https://redirect.github.com/astral-sh/ruff/pull/15883))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xNjQuMSIsInVwZGF0ZWRJblZlciI6IjM5LjE2NC4xIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-02-10 00:25_

---

_Merged by @charliermarsh on 2025-02-10 02:29_

---

_Closed by @charliermarsh on 2025-02-10 02:29_

---

_Branch deleted on 2025-02-10 02:29_

---
