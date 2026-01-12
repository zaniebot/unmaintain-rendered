```yaml
number: 21239
title: Update dependency ruff to v0.14.3
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-11-03T02:59:59Z
updated_at: 2025-11-03T03:13:39Z
url: https://github.com/astral-sh/ruff/pull/21239
synced_at: 2026-01-12T15:57:19Z
```

# Update dependency ruff to v0.14.3

---

_@renovate_

This PR contains the following updates:

| Package | Change | Age | Confidence |
|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.13.3` -> `==0.14.3` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.14.3?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.13.3/0.14.3?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.14.3`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0143)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.14.2...0.14.3)

Released on 2025-10-30.

##### Preview features

- Respect `--output-format` with `--watch` ([#&#8203;21097](https://redirect.github.com/astral-sh/ruff/pull/21097))
- \[`pydoclint`] Fix false positive on explicit exception re-raising (`DOC501`, `DOC502`) ([#&#8203;21011](https://redirect.github.com/astral-sh/ruff/pull/21011))
- \[`pyflakes`] Revert to stable behavior if imports for module lie in alternate branches for `F401` ([#&#8203;20878](https://redirect.github.com/astral-sh/ruff/pull/20878))
- \[`pylint`] Implement `stop-iteration-return` (`PLR1708`) ([#&#8203;20733](https://redirect.github.com/astral-sh/ruff/pull/20733))
- \[`ruff`] Add support for additional eager conversion patterns (`RUF065`) ([#&#8203;20657](https://redirect.github.com/astral-sh/ruff/pull/20657))

##### Bug fixes

- Fix finding keyword range for clause header after statement ending with semicolon ([#&#8203;21067](https://redirect.github.com/astral-sh/ruff/pull/21067))
- Fix syntax error false positive on nested alternative patterns ([#&#8203;21104](https://redirect.github.com/astral-sh/ruff/pull/21104))
- \[`ISC001`] Fix panic when string literals are unclosed ([#&#8203;21034](https://redirect.github.com/astral-sh/ruff/pull/21034))
- \[`flake8-django`] Apply `DJ001` to annotated fields ([#&#8203;20907](https://redirect.github.com/astral-sh/ruff/pull/20907))
- \[`flake8-pyi`] Fix `PYI034` to not trigger on metaclasses (`PYI034`) ([#&#8203;20881](https://redirect.github.com/astral-sh/ruff/pull/20881))
- \[`flake8-type-checking`] Fix `TC003` false positive with `future-annotations` ([#&#8203;21125](https://redirect.github.com/astral-sh/ruff/pull/21125))
- \[`pyflakes`] Fix false positive for `__class__` in lambda expressions within class definitions (`F821`) ([#&#8203;20564](https://redirect.github.com/astral-sh/ruff/pull/20564))
- \[`pyupgrade`] Fix false positive for `TypeVar` with default on Python <3.13 (`UP046`,`UP047`) ([#&#8203;21045](https://redirect.github.com/astral-sh/ruff/pull/21045))

##### Rule changes

- Add missing docstring sections to the numpy list ([#&#8203;20931](https://redirect.github.com/astral-sh/ruff/pull/20931))
- \[`airflow`] Extend `airflow.models..Param` check (`AIR311`) ([#&#8203;21043](https://redirect.github.com/astral-sh/ruff/pull/21043))
- \[`airflow`] Warn that `airflow....DAG.create_dagrun` has been removed (`AIR301`) ([#&#8203;21093](https://redirect.github.com/astral-sh/ruff/pull/21093))
- \[`refurb`] Preserve digit separators in `Decimal` constructor (`FURB157`) ([#&#8203;20588](https://redirect.github.com/astral-sh/ruff/pull/20588))

##### Server

- Avoid sending an unnecessary "clear diagnostics" message for clients supporting pull diagnostics ([#&#8203;21105](https://redirect.github.com/astral-sh/ruff/pull/21105))

##### Documentation

- \[`flake8-bandit`] Fix correct example for `S308` ([#&#8203;21128](https://redirect.github.com/astral-sh/ruff/pull/21128))

##### Other changes

- Clearer error message when `line-length` goes beyond threshold ([#&#8203;21072](https://redirect.github.com/astral-sh/ruff/pull/21072))

##### Contributors

- [@&#8203;danparizher](https://redirect.github.com/danparizher)
- [@&#8203;jvacek](https://redirect.github.com/jvacek)
- [@&#8203;ntBre](https://redirect.github.com/ntBre)
- [@&#8203;augustelalande](https://redirect.github.com/augustelalande)
- [@&#8203;prakhar1144](https://redirect.github.com/prakhar1144)
- [@&#8203;TaKO8Ki](https://redirect.github.com/TaKO8Ki)
- [@&#8203;dylwil3](https://redirect.github.com/dylwil3)
- [@&#8203;fatelei](https://redirect.github.com/fatelei)
- [@&#8203;ShaharNaveh](https://redirect.github.com/ShaharNaveh)
- [@&#8203;Lee-W](https://redirect.github.com/Lee-W)

### [`v0.14.2`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0142)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.14.1...0.14.2)

Released on 2025-10-23.

##### Preview features

- \[`flake8-gettext`] Resolve qualified names and built-in bindings (`INT001`, `INT002`, `INT003`) ([#&#8203;19045](https://redirect.github.com/astral-sh/ruff/pull/19045))

##### Bug fixes

- Avoid reusing nested, interpolated quotes before Python 3.12 ([#&#8203;20930](https://redirect.github.com/astral-sh/ruff/pull/20930))
- Catch syntax errors in nested interpolations before Python 3.12 ([#&#8203;20949](https://redirect.github.com/astral-sh/ruff/pull/20949))
- \[`fastapi`] Handle ellipsis defaults in `FAST002` autofix ([#&#8203;20810](https://redirect.github.com/astral-sh/ruff/pull/20810))
- \[`flake8-simplify`] Skip `SIM911` when unknown arguments are present ([#&#8203;20697](https://redirect.github.com/astral-sh/ruff/pull/20697))
- \[`pyupgrade`] Always parenthesize assignment expressions in fix for `f-string` (`UP032`) ([#&#8203;21003](https://redirect.github.com/astral-sh/ruff/pull/21003))
- \[`pyupgrade`] Fix `UP032` conversion for decimal ints with underscores ([#&#8203;21022](https://redirect.github.com/astral-sh/ruff/pull/21022))
- \[`fastapi`] Skip autofix for keyword and `__debug__` path params (`FAST003`) ([#&#8203;20960](https://redirect.github.com/astral-sh/ruff/pull/20960))

##### Rule changes

- \[`flake8-bugbear`] Skip `B905` and `B912` for fewer than two iterables and no starred arguments ([#&#8203;20998](https://redirect.github.com/astral-sh/ruff/pull/20998))
- \[`ruff`] Use `DiagnosticTag` for more `pyflakes` and `pandas` rules ([#&#8203;20801](https://redirect.github.com/astral-sh/ruff/pull/20801))

##### CLI

- Improve JSON output from `ruff rule` ([#&#8203;20168](https://redirect.github.com/astral-sh/ruff/pull/20168))

##### Documentation

- Add source to testimonial ([#&#8203;20971](https://redirect.github.com/astral-sh/ruff/pull/20971))
- Document when a rule was added ([#&#8203;21035](https://redirect.github.com/astral-sh/ruff/pull/21035))

##### Other changes

- \[syntax-errors] Name is parameter and global ([#&#8203;20426](https://redirect.github.com/astral-sh/ruff/pull/20426))
- \[syntax-errors] Alternative `match` patterns bind different names ([#&#8203;20682](https://redirect.github.com/astral-sh/ruff/pull/20682))

##### Contributors

- [@&#8203;hengky-kurniawan-1](https://redirect.github.com/hengky-kurniawan-1)
- [@&#8203;ShalokShalom](https://redirect.github.com/ShalokShalom)
- [@&#8203;robsdedude](https://redirect.github.com/robsdedude)
- [@&#8203;LoicRiegel](https://redirect.github.com/LoicRiegel)
- [@&#8203;TaKO8Ki](https://redirect.github.com/TaKO8Ki)
- [@&#8203;dylwil3](https://redirect.github.com/dylwil3)
- [@&#8203;11happy](https://redirect.github.com/11happy)
- [@&#8203;ntBre](https://redirect.github.com/ntBre)

### [`v0.14.1`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0141)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.14.0...0.14.1)

Released on 2025-10-16.

##### Preview features

- \[formatter] Remove parentheses around multiple exception types on Python 3.14+ ([#&#8203;20768](https://redirect.github.com/astral-sh/ruff/pull/20768))
- \[`flake8-bugbear`] Omit annotation in preview fix for `B006` ([#&#8203;20877](https://redirect.github.com/astral-sh/ruff/pull/20877))
- \[`flake8-logging-format`] Avoid dropping implicitly concatenated pieces in the `G004` fix ([#&#8203;20793](https://redirect.github.com/astral-sh/ruff/pull/20793))
- \[`pydoclint`] Implement `docstring-extraneous-parameter` (`DOC102`) ([#&#8203;20376](https://redirect.github.com/astral-sh/ruff/pull/20376))
- \[`pyupgrade`] Extend `UP019` to detect `typing_extensions.Text` (`UP019`) ([#&#8203;20825](https://redirect.github.com/astral-sh/ruff/pull/20825))
- \[`pyupgrade`] Fix false negative for `TypeVar` with default argument in `non-pep695-generic-class` (`UP046`) ([#&#8203;20660](https://redirect.github.com/astral-sh/ruff/pull/20660))

##### Bug fixes

- Fix false negatives in `Truthiness::from_expr` for lambdas, generators, and f-strings ([#&#8203;20704](https://redirect.github.com/astral-sh/ruff/pull/20704))
- Fix syntax error false positives for escapes and quotes in f-strings ([#&#8203;20867](https://redirect.github.com/astral-sh/ruff/pull/20867))
- Fix syntax error false positives on parenthesized context managers ([#&#8203;20846](https://redirect.github.com/astral-sh/ruff/pull/20846))
- \[`fastapi`] Fix false positives for path parameters that FastAPI doesn't recognize (`FAST003`) ([#&#8203;20687](https://redirect.github.com/astral-sh/ruff/pull/20687))
- \[`flake8-pyi`] Fix operator precedence by adding parentheses when needed (`PYI061`) ([#&#8203;20508](https://redirect.github.com/astral-sh/ruff/pull/20508))
- \[`ruff`] Suppress diagnostic for f-string interpolations with debug text (`RUF010`) ([#&#8203;20525](https://redirect.github.com/astral-sh/ruff/pull/20525))

##### Rule changes

- \[`airflow`] Add warning to `airflow.datasets.DatasetEvent` usage (`AIR301`) ([#&#8203;20551](https://redirect.github.com/astral-sh/ruff/pull/20551))
- \[`flake8-bugbear`] Mark `B905` and `B912` fixes as unsafe ([#&#8203;20695](https://redirect.github.com/astral-sh/ruff/pull/20695))
- Use `DiagnosticTag` for more rules - changes display in editors ([#&#8203;20758](https://redirect.github.com/astral-sh/ruff/pull/20758),[#&#8203;20734](https://redirect.github.com/astral-sh/ruff/pull/20734))

##### Documentation

- Update Python compatibility from 3.13 to 3.14 in README.md ([#&#8203;20852](https://redirect.github.com/astral-sh/ruff/pull/20852))
- Update `lint.flake8-type-checking.quoted-annotations` docs ([#&#8203;20765](https://redirect.github.com/astral-sh/ruff/pull/20765))
- Update setup instructions for Zed 0.208.0+ ([#&#8203;20902](https://redirect.github.com/astral-sh/ruff/pull/20902))
- \[`flake8-datetimez`] Clarify docs for several rules ([#&#8203;20778](https://redirect.github.com/astral-sh/ruff/pull/20778))
- Fix typo in `RUF015` description ([#&#8203;20873](https://redirect.github.com/astral-sh/ruff/pull/20873))

##### Other changes

- Reduce binary size ([#&#8203;20863](https://redirect.github.com/astral-sh/ruff/pull/20863))
- Improved error recovery for unclosed strings (including f- and t-strings) ([#&#8203;20848](https://redirect.github.com/astral-sh/ruff/pull/20848))

##### Contributors

- [@&#8203;ntBre](https://redirect.github.com/ntBre)
- [@&#8203;Paillat-dev](https://redirect.github.com/Paillat-dev)
- [@&#8203;terror](https://redirect.github.com/terror)
- [@&#8203;pieterh-oai](https://redirect.github.com/pieterh-oai)
- [@&#8203;MichaReiser](https://redirect.github.com/MichaReiser)
- [@&#8203;TaKO8Ki](https://redirect.github.com/TaKO8Ki)
- [@&#8203;ageorgou](https://redirect.github.com/ageorgou)
- [@&#8203;danparizher](https://redirect.github.com/danparizher)
- [@&#8203;mgaitan](https://redirect.github.com/mgaitan)
- [@&#8203;augustelalande](https://redirect.github.com/augustelalande)
- [@&#8203;dylwil3](https://redirect.github.com/dylwil3)
- [@&#8203;Lee-W](https://redirect.github.com/Lee-W)
- [@&#8203;injust](https://redirect.github.com/injust)
- [@&#8203;CarrotManMatt](https://redirect.github.com/CarrotManMatt)

### [`v0.14.0`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0140)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.13.3...0.14.0)

Released on 2025-10-07.

##### Breaking changes

- Update default and latest Python versions for 3.14 ([#&#8203;20725](https://redirect.github.com/astral-sh/ruff/pull/20725))

##### Preview features

- \[`flake8-bugbear`] Include certain guaranteed-mutable expressions: tuples, generators, and assignment expressions (`B006`) ([#&#8203;20024](https://redirect.github.com/astral-sh/ruff/pull/20024))
- \[`refurb`] Add fixes for `FURB101` and `FURB103` ([#&#8203;20520](https://redirect.github.com/astral-sh/ruff/pull/20520))
- \[`ruff`] Extend `FA102` with listed PEP 585-compatible APIs ([#&#8203;20659](https://redirect.github.com/astral-sh/ruff/pull/20659))

##### Bug fixes

- \[`flake8-annotations`] Fix return type annotations to handle shadowed builtin symbols (`ANN201`, `ANN202`, `ANN204`, `ANN205`, `ANN206`) ([#&#8203;20612](https://redirect.github.com/astral-sh/ruff/pull/20612))
- \[`flynt`] Fix f-string quoting for mixed quote joiners (`FLY002`) ([#&#8203;20662](https://redirect.github.com/astral-sh/ruff/pull/20662))
- \[`isort`] Fix inserting required imports before future imports (`I002`) ([#&#8203;20676](https://redirect.github.com/astral-sh/ruff/pull/20676))
- \[`ruff`] Handle argfile expansion errors gracefully ([#&#8203;20691](https://redirect.github.com/astral-sh/ruff/pull/20691))
- \[`ruff`] Skip `RUF051` if `else`/`elif` block is present ([#&#8203;20705](https://redirect.github.com/astral-sh/ruff/pull/20705))
- \[`ruff`] Improve handling of intermixed comments inside from-imports ([#&#8203;20561](https://redirect.github.com/astral-sh/ruff/pull/20561))

##### Documentation

- \[`flake8-comprehensions`] Clarify fix safety documentation (`C413`) ([#&#8203;20640](https://redirect.github.com/astral-sh/ruff/pull/20640))

##### Contributors

- [@&#8203;danparizher](https://redirect.github.com/danparizher)
- [@&#8203;terror](https://redirect.github.com/terror)
- [@&#8203;TaKO8Ki](https://redirect.github.com/TaKO8Ki)
- [@&#8203;ntBre](https://redirect.github.com/ntBre)
- [@&#8203;njhearp](https://redirect.github.com/njhearp)
- [@&#8203;amyreese](https://redirect.github.com/amyreese)
- [@&#8203;IDrokin117](https://redirect.github.com/IDrokin117)
- [@&#8203;chirizxc](https://redirect.github.com/chirizxc)

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS4xNTkuNCIsInVwZGF0ZWRJblZlciI6IjQxLjE1OS40IiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-11-03 03:00_

---

_Merged by @MichaReiser on 2025-11-03 03:13_

---

_Closed by @MichaReiser on 2025-11-03 03:13_

---

_Branch deleted on 2025-11-03 03:13_

---
