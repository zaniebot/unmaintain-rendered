```yaml
number: 15432
title: Update dependency ruff to v0.9.1
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-01-11T16:14:59Z
updated_at: 2025-01-11T17:21:13Z
url: https://github.com/astral-sh/ruff/pull/15432
synced_at: 2026-01-12T15:55:51Z
```

# Update dependency ruff to v0.9.1

---

_@renovate_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.8.6` -> `==0.9.1` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.9.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.9.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.8.6/0.9.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.8.6/0.9.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.9.1`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#091)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.9.0...0.9.1)

##### Preview features

-   \[`pycodestyle`] Run `too-many-newlines-at-end-of-file` on each cell in notebooks (`W391`) ([#&#8203;15308](https://redirect.github.com/astral-sh/ruff/pull/15308))
-   \[`ruff`] Omit diagnostic for shadowed private function parameters in `used-dummy-variable` (`RUF052`) ([#&#8203;15376](https://redirect.github.com/astral-sh/ruff/pull/15376))

##### Rule changes

-   \[`flake8-bugbear`] Improve `assert-raises-exception` message (`B017`) ([#&#8203;15389](https://redirect.github.com/astral-sh/ruff/pull/15389))

##### Formatter

-   Preserve trailing end-of line comments for the last string literal in implicitly concatenated strings ([#&#8203;15378](https://redirect.github.com/astral-sh/ruff/pull/15378))

##### Server

-   Fix a bug where the server and client notebooks were out of sync after reordering cells ([#&#8203;15398](https://redirect.github.com/astral-sh/ruff/pull/15398))

##### Bug fixes

-   \[`flake8-pie`] Correctly remove wrapping parentheses (`PIE800`) ([#&#8203;15394](https://redirect.github.com/astral-sh/ruff/pull/15394))
-   \[`pyupgrade`] Handle comments and multiline expressions correctly (`UP037`) ([#&#8203;15337](https://redirect.github.com/astral-sh/ruff/pull/15337))

### [`v0.9.0`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#090)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.8.6...0.9.0)

Check out the [blog post](https://astral.sh/blog/ruff-v0.9.0) for a migration guide and overview of the changes!

##### Breaking changes

Ruff now formats your code according to the 2025 style guide. As a result, your code might now get formatted differently. See the formatter section for a detailed list of changes.

This release doesn’t remove or remap any existing stable rules.

##### Stabilization

The following rules have been stabilized and are no longer in preview:

-   [`stdlib-module-shadowing`](https://docs.astral.sh/ruff/rules/stdlib-module-shadowing/) (`A005`).
    This rule has also been renamed: previously, it was called `builtin-module-shadowing`.
-   [`builtin-lambda-argument-shadowing`](https://docs.astral.sh/ruff/rules/builtin-lambda-argument-shadowing/) (`A006`)
-   [`slice-to-remove-prefix-or-suffix`](https://docs.astral.sh/ruff/rules/slice-to-remove-prefix-or-suffix/) (`FURB188`)
-   [`boolean-chained-comparison`](https://docs.astral.sh/ruff/rules/boolean-chained-comparison/) (`PLR1716`)
-   [`decimal-from-float-literal`](https://docs.astral.sh/ruff/rules/decimal-from-float-literal/) (`RUF032`)
-   [`post-init-default`](https://docs.astral.sh/ruff/rules/post-init-default/) (`RUF033`)
-   [`useless-if-else`](https://docs.astral.sh/ruff/rules/useless-if-else/) (`RUF034`)

The following behaviors have been stabilized:

-   [`pytest-parametrize-names-wrong-type`](https://docs.astral.sh/ruff/rules/pytest-parametrize-names-wrong-type/) (`PT006`): Detect [`pytest.parametrize`](https://docs.pytest.org/en/7.1.x/how-to/parametrize.html#parametrize) calls outside decorators and calls with keyword arguments.
-   [`module-import-not-at-top-of-file`](https://docs.astral.sh/ruff/rules/module-import-not-at-top-of-file/) (`E402`): Ignore [`pytest.importorskip`](https://docs.pytest.org/en/7.1.x/reference/reference.html#pytest-importorskip) calls between import statements.
-   [`mutable-dataclass-default`](https://docs.astral.sh/ruff/rules/mutable-dataclass-default/) (`RUF008`) and [`function-call-in-dataclass-default-argument`](https://docs.astral.sh/ruff/rules/function-call-in-dataclass-default-argument/) (`RUF009`): Add support for [`attrs`](https://www.attrs.org/en/stable/).
-   [`bad-version-info-comparison`](https://docs.astral.sh/ruff/rules/bad-version-info-comparison/) (`PYI006`): Extend the rule to check non-stub files.

The following fixes or improvements to fixes have been stabilized:

-   [`redundant-numeric-union`](https://docs.astral.sh/ruff/rules/redundant-numeric-union/) (`PYI041`)
-   [`duplicate-union-members`](https://docs.astral.sh/ruff/rules/duplicate-union-member/) (`PYI016`)

##### Formatter

This release introduces the new 2025 stable style ([#&#8203;13371](https://redirect.github.com/astral-sh/ruff/issues/13371)), stabilizing the following changes:

-   Format expressions in f-string elements ([#&#8203;7594](https://redirect.github.com/astral-sh/ruff/issues/7594))
-   Alternate quotes for strings inside f-strings ([#&#8203;13860](https://redirect.github.com/astral-sh/ruff/pull/13860))
-   Preserve the casing of hex codes in f-string debug expressions ([#&#8203;14766](https://redirect.github.com/astral-sh/ruff/issues/14766))
-   Choose the quote style for each string literal in an implicitly concatenated f-string rather than for the entire string ([#&#8203;13539](https://redirect.github.com/astral-sh/ruff/pull/13539))
-   Automatically join an implicitly concatenated string into a single string literal if it fits on a single line ([#&#8203;9457](https://redirect.github.com/astral-sh/ruff/issues/9457))
-   Remove the [`ISC001`](https://docs.astral.sh/ruff/rules/single-line-implicit-string-concatenation/) incompatibility warning ([#&#8203;15123](https://redirect.github.com/astral-sh/ruff/pull/15123))
-   Prefer parenthesizing the `assert` message over breaking the assertion expression ([#&#8203;9457](https://redirect.github.com/astral-sh/ruff/issues/9457))
-   Automatically parenthesize over-long `if` guards in `match` `case` clauses ([#&#8203;13513](https://redirect.github.com/astral-sh/ruff/pull/13513))
-   More consistent formatting for `match` `case` patterns ([#&#8203;6933](https://redirect.github.com/astral-sh/ruff/issues/6933))
-   Avoid unnecessary parentheses around return type annotations ([#&#8203;13381](https://redirect.github.com/astral-sh/ruff/pull/13381))
-   Keep the opening parentheses on the same line as the `if` keyword for comprehensions where the condition has a leading comment ([#&#8203;12282](https://redirect.github.com/astral-sh/ruff/pull/12282))
-   More consistent formatting for `with` statements with a single context manager for Python 3.8 or older ([#&#8203;10276](https://redirect.github.com/astral-sh/ruff/pull/10276))
-   Correctly calculate the line-width for code blocks in docstrings when using `max-doc-code-line-length = "dynamic"` ([#&#8203;13523](https://redirect.github.com/astral-sh/ruff/pull/13523))

##### Preview features

-   \[`flake8-bugbear`] Implement `class-as-data-structure` (`B903`) ([#&#8203;9601](https://redirect.github.com/astral-sh/ruff/pull/9601))
-   \[`flake8-type-checking`] Apply `quoted-type-alias` more eagerly in `TYPE_CHECKING` blocks and ignore it in stubs (`TC008`) ([#&#8203;15180](https://redirect.github.com/astral-sh/ruff/pull/15180))
-   \[`pylint`] Ignore `eq-without-hash` in stub files (`PLW1641`) ([#&#8203;15310](https://redirect.github.com/astral-sh/ruff/pull/15310))
-   \[`pyupgrade`] Split `UP007` into two individual rules: `UP007` for `Union` and `UP045` for `Optional` (`UP007`, `UP045`) ([#&#8203;15313](https://redirect.github.com/astral-sh/ruff/pull/15313))
-   \[`ruff`] New rule that detects classes that are both an enum and a `dataclass` (`RUF049`) ([#&#8203;15299](https://redirect.github.com/astral-sh/ruff/pull/15299))
-   \[`ruff`] Recode `RUF025` to `RUF037` (`RUF037`) ([#&#8203;15258](https://redirect.github.com/astral-sh/ruff/pull/15258))

##### Rule changes

-   \[`flake8-builtins`] Ignore [`stdlib-module-shadowing`](https://docs.astral.sh/ruff/rules/stdlib-module-shadowing/) in stub files(`A005`) ([#&#8203;15350](https://redirect.github.com/astral-sh/ruff/pull/15350))
-   \[`flake8-return`] Add support for functions returning `typing.Never` (`RET503`) ([#&#8203;15298](https://redirect.github.com/astral-sh/ruff/pull/15298))

##### Server

-   Improve the observability by removing the need for the ["trace" value](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#traceValue) to turn on or off logging. The server logging is solely controlled using the [`logLevel` server setting](https://docs.astral.sh/ruff/editors/settings/#loglevel)
    which defaults to `info`. This addresses the issue where users were notified about an error and told to consult the log, but it didn’t contain any messages. ([#&#8203;15232](https://redirect.github.com/astral-sh/ruff/pull/15232))
-   Ignore diagnostics from other sources for code action requests ([#&#8203;15373](https://redirect.github.com/astral-sh/ruff/pull/15373))

##### CLI

-   Improve the error message for `--config key=value` when the `key` is for a table and it’s a simple `value`

##### Bug fixes

-   \[`eradicate`] Ignore metadata blocks directly followed by normal blocks (`ERA001`) ([#&#8203;15330](https://redirect.github.com/astral-sh/ruff/pull/15330))
-   \[`flake8-django`] Recognize other magic methods (`DJ012`) ([#&#8203;15365](https://redirect.github.com/astral-sh/ruff/pull/15365))
-   \[`pycodestyle`] Avoid false positives related to type aliases (`E252`) ([#&#8203;15356](https://redirect.github.com/astral-sh/ruff/pull/15356))
-   \[`pydocstyle`] Avoid treating newline-separated sections as sub-sections (`D405`) ([#&#8203;15311](https://redirect.github.com/astral-sh/ruff/pull/15311))
-   \[`pyflakes`] Remove call when removing final argument from `format` (`F523`) ([#&#8203;15309](https://redirect.github.com/astral-sh/ruff/pull/15309))
-   \[`refurb`] Mark fix as unsafe when the right-hand side is a string (`FURB171`) ([#&#8203;15273](https://redirect.github.com/astral-sh/ruff/pull/15273))
-   \[`ruff`] Treat `)` as a regex metacharacter (`RUF043`, `RUF055`) ([#&#8203;15318](https://redirect.github.com/astral-sh/ruff/pull/15318))
-   \[`ruff`] Parenthesize the `int`-call argument when removing the `int` call would change semantics (`RUF046`) ([#&#8203;15277](https://redirect.github.com/astral-sh/ruff/pull/15277))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS45Mi4wIiwidXBkYXRlZEluVmVyIjoiMzkuOTIuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-01-11 16:14_

---

_@MichaReiser approved on 2025-01-11 17:10_

---

_Merged by @MichaReiser on 2025-01-11 17:18_

---

_Closed by @MichaReiser on 2025-01-11 17:18_

---

_Branch deleted on 2025-01-11 17:18_

---

_Comment by @github-actions[bot] on 2025-01-11 17:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>




---
