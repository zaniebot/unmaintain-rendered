```yaml
number: 12823
title: Update dependency ruff to v0.5.7 - autoclosed
type: pull_request
state: closed
author: renovate
labels:
  - internal
assignees: []
base: main
head: renovate/ruff-0.x
created_at: 2024-08-12T02:00:54Z
updated_at: 2024-08-13T17:16:20Z
url: https://github.com/astral-sh/ruff/pull/12823
synced_at: 2026-01-10T21:38:32Z
```

# Update dependency ruff to v0.5.7 - autoclosed

---

_Pull request opened by @renovate on 2024-08-12 02:00_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://togithub.com/astral-sh/ruff), [changelog](https://togithub.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.5.0` -> `==0.5.7` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.5.7?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.5.7?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.5.0/0.5.7?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.5.0/0.5.7?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.5.7`](https://togithub.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#057)

[Compare Source](https://togithub.com/astral-sh/ruff/compare/0.5.6...0.5.7)

##### Preview features

-   \[`flake8-comprehensions`] Account for list and set comprehensions in `unnecessary-literal-within-tuple-call` (`C409`) ([#&#8203;12657](https://togithub.com/astral-sh/ruff/pull/12657))
-   \[`flake8-pyi`] Add autofix for `future-annotations-in-stub` (`PYI044`) ([#&#8203;12676](https://togithub.com/astral-sh/ruff/pull/12676))
-   \[`flake8-return`] Avoid syntax error when auto-fixing `RET505` with mixed indentation (space and tabs) ([#&#8203;12740](https://togithub.com/astral-sh/ruff/pull/12740))
-   \[`pydoclint`] Add `docstring-missing-yields` (`DOC402`) and `docstring-extraneous-yields` (`DOC403`) ([#&#8203;12538](https://togithub.com/astral-sh/ruff/pull/12538))
-   \[`pydoclint`] Avoid `DOC201` if docstring begins with "Return", "Returns", "Yield", or "Yields" ([#&#8203;12675](https://togithub.com/astral-sh/ruff/pull/12675))
-   \[`pydoclint`] Deduplicate collected exceptions after traversing function bodies (`DOC501`) ([#&#8203;12642](https://togithub.com/astral-sh/ruff/pull/12642))
-   \[`pydoclint`] Ignore `DOC` errors for stub functions ([#&#8203;12651](https://togithub.com/astral-sh/ruff/pull/12651))
-   \[`pydoclint`] Teach rules to understand reraised exceptions as being explicitly raised (`DOC501`, `DOC502`) ([#&#8203;12639](https://togithub.com/astral-sh/ruff/pull/12639))
-   \[`ruff`] Implement `incorrectly-parenthesized-tuple-in-subscript` (`RUF031`) ([#&#8203;12480](https://togithub.com/astral-sh/ruff/pull/12480))
-   \[`ruff`] Mark `RUF023` fix as unsafe if `__slots__` is not a set and the binding is used elsewhere ([#&#8203;12692](https://togithub.com/astral-sh/ruff/pull/12692))

##### Rule changes

-   \[`refurb`] Add autofix for `implicit-cwd` (`FURB177`) ([#&#8203;12708](https://togithub.com/astral-sh/ruff/pull/12708))
-   \[`ruff`] Add autofix for `zip-instead-of-pairwise` (`RUF007`) ([#&#8203;12663](https://togithub.com/astral-sh/ruff/pull/12663))
-   \[`tryceratops`] Add `BaseException` to `raise-vanilla-class` rule (`TRY002`) ([#&#8203;12620](https://togithub.com/astral-sh/ruff/pull/12620))

##### Server

-   Ignore non-file workspace URL; Ruff will display a warning notification in this case ([#&#8203;12725](https://togithub.com/astral-sh/ruff/pull/12725))

##### CLI

-   Fix cache invalidation for nested `pyproject.toml` files ([#&#8203;12727](https://togithub.com/astral-sh/ruff/pull/12727))

##### Bug fixes

-   \[`flake8-async`] Fix false positives with multiple `async with` items (`ASYNC100`) ([#&#8203;12643](https://togithub.com/astral-sh/ruff/pull/12643))
-   \[`flake8-bandit`] Avoid false-positives for list concatenations in SQL construction (`S608`) ([#&#8203;12720](https://togithub.com/astral-sh/ruff/pull/12720))
-   \[`flake8-bugbear`] Treat `return` as equivalent to `break` (`B909`) ([#&#8203;12646](https://togithub.com/astral-sh/ruff/pull/12646))
-   \[`flake8-comprehensions`] Set comprehensions not a violation for `sum` in `unnecessary-comprehension-in-call` (`C419`) ([#&#8203;12691](https://togithub.com/astral-sh/ruff/pull/12691))
-   \[`flake8-simplify`] Parenthesize conditions based on precedence when merging if arms (`SIM114`) ([#&#8203;12737](https://togithub.com/astral-sh/ruff/pull/12737))
-   \[`pydoclint`] Try both 'Raises' section styles when convention is unspecified (`DOC501`) ([#&#8203;12649](https://togithub.com/astral-sh/ruff/pull/12649))

### [`v0.5.6`](https://togithub.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#056)

[Compare Source](https://togithub.com/astral-sh/ruff/compare/0.5.5...0.5.6)

Ruff 0.5.6 automatically enables linting and formatting of notebooks in *preview mode*.
You can opt-out of this behavior by adding `*.ipynb` to the `extend-exclude` setting.

```toml
[tool.ruff]
extend-exclude = ["*.ipynb"]
```

##### Preview features

-   Enable notebooks by default in preview mode ([#&#8203;12621](https://togithub.com/astral-sh/ruff/pull/12621))
-   \[`flake8-builtins`] Implement import, lambda, and module shadowing ([#&#8203;12546](https://togithub.com/astral-sh/ruff/pull/12546))
-   \[`pydoclint`] Add `docstring-missing-returns` (`DOC201`) and `docstring-extraneous-returns` (`DOC202`) ([#&#8203;12485](https://togithub.com/astral-sh/ruff/pull/12485))

##### Rule changes

-   \[`flake8-return`] Exempt cached properties and other property-like decorators from explicit return rule (`RET501`) ([#&#8203;12563](https://togithub.com/astral-sh/ruff/pull/12563))

##### Server

-   Make server panic hook more error resilient ([#&#8203;12610](https://togithub.com/astral-sh/ruff/pull/12610))
-   Use `$/logTrace` for server trace logs in Zed and VS Code ([#&#8203;12564](https://togithub.com/astral-sh/ruff/pull/12564))
-   Keep track of deleted cells for reorder change request ([#&#8203;12575](https://togithub.com/astral-sh/ruff/pull/12575))

##### Configuration

-   \[`flake8-implicit-str-concat`] Always allow explicit multi-line concatenations when implicit concatenations are banned ([#&#8203;12532](https://togithub.com/astral-sh/ruff/pull/12532))

##### Bug fixes

-   \[`flake8-async`] Avoid flagging `asyncio.timeout`s as unused when the context manager includes `asyncio.TaskGroup` ([#&#8203;12605](https://togithub.com/astral-sh/ruff/pull/12605))
-   \[`flake8-slots`] Avoid recommending `__slots__` for classes that inherit from more than `namedtuple` ([#&#8203;12531](https://togithub.com/astral-sh/ruff/pull/12531))
-   \[`isort`] Avoid marking required imports as unused ([#&#8203;12537](https://togithub.com/astral-sh/ruff/pull/12537))
-   \[`isort`] Preserve trailing inline comments on import-from statements ([#&#8203;12498](https://togithub.com/astral-sh/ruff/pull/12498))
-   \[`pycodestyle`] Add newlines before comments (`E305`) ([#&#8203;12606](https://togithub.com/astral-sh/ruff/pull/12606))
-   \[`pycodestyle`] Don't attach comments with mismatched indents ([#&#8203;12604](https://togithub.com/astral-sh/ruff/pull/12604))
-   \[`pyflakes`] Fix preview-mode bugs in `F401` when attempting to autofix unused first-party submodule imports in an `__init__.py` file ([#&#8203;12569](https://togithub.com/astral-sh/ruff/pull/12569))
-   \[`pylint`] Respect start index in `unnecessary-list-index-lookup` ([#&#8203;12603](https://togithub.com/astral-sh/ruff/pull/12603))
-   \[`pyupgrade`] Avoid recommending no-argument super in `slots=True` dataclasses ([#&#8203;12530](https://togithub.com/astral-sh/ruff/pull/12530))
-   \[`pyupgrade`] Use colon rather than dot formatting for integer-only types ([#&#8203;12534](https://togithub.com/astral-sh/ruff/pull/12534))
-   Fix NFKC normalization bug when removing unused imports ([#&#8203;12571](https://togithub.com/astral-sh/ruff/pull/12571))

##### Other changes

-   Consider more stdlib decorators to be property-like ([#&#8203;12583](https://togithub.com/astral-sh/ruff/pull/12583))
-   Improve handling of metaclasses in various linter rules ([#&#8203;12579](https://togithub.com/astral-sh/ruff/pull/12579))
-   Improve consistency between linter rules in determining whether a function is property ([#&#8203;12581](https://togithub.com/astral-sh/ruff/pull/12581))

### [`v0.5.5`](https://togithub.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#055)

[Compare Source](https://togithub.com/astral-sh/ruff/compare/0.5.4...0.5.5)

##### Preview features

-   \[`fastapi`] Implement `fastapi-redundant-response-model` (`FAST001`) and `fastapi-non-annotated-dependency`(`FAST002`) ([#&#8203;11579](https://togithub.com/astral-sh/ruff/pull/11579))
-   \[`pydoclint`] Implement `docstring-missing-exception` (`DOC501`) and `docstring-extraneous-exception` (`DOC502`) ([#&#8203;11471](https://togithub.com/astral-sh/ruff/pull/11471))

##### Rule changes

-   \[`numpy`] Fix NumPy 2.0 rule for `np.alltrue` and `np.sometrue` ([#&#8203;12473](https://togithub.com/astral-sh/ruff/pull/12473))
-   \[`numpy`] Ignore `NPY201` inside `except` blocks for compatibility with older numpy versions ([#&#8203;12490](https://togithub.com/astral-sh/ruff/pull/12490))
-   \[`pep8-naming`] Avoid applying `ignore-names` to `self` and `cls` function names (`N804`, `N805`) ([#&#8203;12497](https://togithub.com/astral-sh/ruff/pull/12497))

##### Formatter

-   Fix incorrect placement of leading function comment with type params ([#&#8203;12447](https://togithub.com/astral-sh/ruff/pull/12447))

##### Server

-   Do not bail code action resolution when a quick fix is requested ([#&#8203;12462](https://togithub.com/astral-sh/ruff/pull/12462))

##### Bug fixes

-   Fix `Ord` implementation of `cmp_fix` ([#&#8203;12471](https://togithub.com/astral-sh/ruff/pull/12471))
-   Raise syntax error for unparenthesized generator expression in multi-argument call ([#&#8203;12445](https://togithub.com/astral-sh/ruff/pull/12445))
-   \[`pydoclint`] Fix panic in `DOC501` reported in [#&#8203;12428](https://togithub.com/astral-sh/ruff/pull/12428) ([#&#8203;12435](https://togithub.com/astral-sh/ruff/pull/12435))
-   \[`flake8-bugbear`] Allow singleton tuples with starred expressions in `B013` ([#&#8203;12484](https://togithub.com/astral-sh/ruff/pull/12484))

##### Documentation

-   Add Eglot setup guide for Emacs editor ([#&#8203;12426](https://togithub.com/astral-sh/ruff/pull/12426))
-   Add note about the breaking change in `nvim-lspconfig` ([#&#8203;12507](https://togithub.com/astral-sh/ruff/pull/12507))
-   Add note to include notebook files for native server ([#&#8203;12449](https://togithub.com/astral-sh/ruff/pull/12449))
-   Add setup docs for Zed editor ([#&#8203;12501](https://togithub.com/astral-sh/ruff/pull/12501))

### [`v0.5.4`](https://togithub.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#054)

[Compare Source](https://togithub.com/astral-sh/ruff/compare/0.5.3...0.5.4)

##### Rule changes

-   \[`ruff`] Rename `RUF007` to `zip-instead-of-pairwise` ([#&#8203;12399](https://togithub.com/astral-sh/ruff/pull/12399))

##### Bug fixes

-   \[`flake8-builtins`] Avoid shadowing diagnostics for `@override` methods ([#&#8203;12415](https://togithub.com/astral-sh/ruff/pull/12415))
-   \[`flake8-comprehensions`] Insert parentheses for multi-argument generators ([#&#8203;12422](https://togithub.com/astral-sh/ruff/pull/12422))
-   \[`pydocstyle`] Handle escaped docstrings within docstring (`D301`) ([#&#8203;12192](https://togithub.com/astral-sh/ruff/pull/12192))

##### Documentation

-   Fix GitHub link to Neovim setup ([#&#8203;12410](https://togithub.com/astral-sh/ruff/pull/12410))
-   Fix `output-format` default in settings reference ([#&#8203;12409](https://togithub.com/astral-sh/ruff/pull/12409))

### [`v0.5.3`](https://togithub.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#053)

[Compare Source](https://togithub.com/astral-sh/ruff/compare/0.5.2...0.5.3)

**Ruff 0.5.3 marks the stable release of the Ruff language server and introduces revamped
[documentation](https://docs.astral.sh/ruff/editors), including [setup guides for your editor of
choice](https://docs.astral.sh/ruff/editors/setup) and [the language server
itself](https://docs.astral.sh/ruff/editors/settings)**.

##### Preview features

-   Formatter: Insert empty line between suite and alternative branch after function/class definition ([#&#8203;12294](https://togithub.com/astral-sh/ruff/pull/12294))
-   \[`pyupgrade`] Implement `unnecessary-default-type-args` (`UP043`) ([#&#8203;12371](https://togithub.com/astral-sh/ruff/pull/12371))

##### Rule changes

-   \[`flake8-bugbear`] Detect enumerate iterations in `loop-iterator-mutation` (`B909`) ([#&#8203;12366](https://togithub.com/astral-sh/ruff/pull/12366))
-   \[`flake8-bugbear`] Remove `discard`, `remove`, and `pop` allowance for `loop-iterator-mutation` (`B909`) ([#&#8203;12365](https://togithub.com/astral-sh/ruff/pull/12365))
-   \[`pylint`] Allow `repeated-equality-comparison` for mixed operations (`PLR1714`) ([#&#8203;12369](https://togithub.com/astral-sh/ruff/pull/12369))
-   \[`pylint`] Ignore `self` and `cls` when counting arguments (`PLR0913`) ([#&#8203;12367](https://togithub.com/astral-sh/ruff/pull/12367))
-   \[`pylint`] Use UTF-8 as default encoding in `unspecified-encoding` fix (`PLW1514`) ([#&#8203;12370](https://togithub.com/astral-sh/ruff/pull/12370))

##### Server

-   Build settings index in parallel for the native server ([#&#8203;12299](https://togithub.com/astral-sh/ruff/pull/12299))
-   Use fallback settings when indexing the project ([#&#8203;12362](https://togithub.com/astral-sh/ruff/pull/12362))
-   Consider `--preview` flag for `server` subcommand for the linter and formatter ([#&#8203;12208](https://togithub.com/astral-sh/ruff/pull/12208))

##### Bug fixes

-   \[`flake8-comprehensions`] Allow additional arguments for `sum` and `max` comprehensions (`C419`) ([#&#8203;12364](https://togithub.com/astral-sh/ruff/pull/12364))
-   \[`pylint`] Avoid dropping extra boolean operations in `repeated-equality-comparison` (`PLR1714`) ([#&#8203;12368](https://togithub.com/astral-sh/ruff/pull/12368))
-   \[`pylint`] Consider expression before statement when determining binding kind (`PLR1704`) ([#&#8203;12346](https://togithub.com/astral-sh/ruff/pull/12346))

##### Documentation

-   Add docs for Ruff language server ([#&#8203;12344](https://togithub.com/astral-sh/ruff/pull/12344))
-   Migrate to standalone docs repo ([#&#8203;12341](https://togithub.com/astral-sh/ruff/pull/12341))
-   Update versioning policy for editor integration ([#&#8203;12375](https://togithub.com/astral-sh/ruff/pull/12375))

##### Other changes

-   Publish Wasm API to npm ([#&#8203;12317](https://togithub.com/astral-sh/ruff/pull/12317))

### [`v0.5.2`](https://togithub.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#052)

[Compare Source](https://togithub.com/astral-sh/ruff/compare/0.5.1...0.5.2)

##### Preview features

-   Use `space` separator before parenthesized expressions in comprehensions with leading comments ([#&#8203;12282](https://togithub.com/astral-sh/ruff/pull/12282))
-   \[`flake8-async`] Update `ASYNC100` to include `anyio` and `asyncio` ([#&#8203;12221](https://togithub.com/astral-sh/ruff/pull/12221))
-   \[`flake8-async`] Update `ASYNC109` to include `anyio` and `asyncio` ([#&#8203;12236](https://togithub.com/astral-sh/ruff/pull/12236))
-   \[`flake8-async`] Update `ASYNC110` to include `anyio` and `asyncio` ([#&#8203;12261](https://togithub.com/astral-sh/ruff/pull/12261))
-   \[`flake8-async`] Update `ASYNC115` to include `anyio` and `asyncio` ([#&#8203;12262](https://togithub.com/astral-sh/ruff/pull/12262))
-   \[`flake8-async`] Update `ASYNC116` to include `anyio` and `asyncio` ([#&#8203;12266](https://togithub.com/astral-sh/ruff/pull/12266))

##### Rule changes

-   \[`flake8-return`] Exempt properties from explicit return rule (`RET501`) ([#&#8203;12243](https://togithub.com/astral-sh/ruff/pull/12243))
-   \[`numpy`] Add `np.NAN`-to-`np.nan` diagnostic ([#&#8203;12292](https://togithub.com/astral-sh/ruff/pull/12292))
-   \[`refurb`] Make `list-reverse-copy` an unsafe fix ([#&#8203;12303](https://togithub.com/astral-sh/ruff/pull/12303))

##### Server

-   Consider `include` and `extend-include` settings in native server ([#&#8203;12252](https://togithub.com/astral-sh/ruff/pull/12252))
-   Include nested configurations in settings reloading ([#&#8203;12253](https://togithub.com/astral-sh/ruff/pull/12253))

##### CLI

-   Omit code frames for fixes with empty ranges ([#&#8203;12304](https://togithub.com/astral-sh/ruff/pull/12304))
-   Warn about formatter incompatibility for `D203` ([#&#8203;12238](https://togithub.com/astral-sh/ruff/pull/12238))

##### Bug fixes

-   Make cache-write failures non-fatal on Windows ([#&#8203;12302](https://togithub.com/astral-sh/ruff/pull/12302))
-   Treat `not` operations as boolean tests ([#&#8203;12301](https://togithub.com/astral-sh/ruff/pull/12301))
-   \[`flake8-bandit`] Avoid `S310` violations for HTTP-safe f-strings ([#&#8203;12305](https://togithub.com/astral-sh/ruff/pull/12305))
-   \[`flake8-bandit`] Support explicit string concatenations in S310 HTTP detection ([#&#8203;12315](https://togithub.com/astral-sh/ruff/pull/12315))
-   \[`flake8-bandit`] fix S113 false positive for httpx without `timeout` argument ([#&#8203;12213](https://togithub.com/astral-sh/ruff/pull/12213))
-   \[`pycodestyle`] Remove "non-obvious" allowance for E721 ([#&#8203;12300](https://togithub.com/astral-sh/ruff/pull/12300))
-   \[`pyflakes`] Consider `with` blocks as single-item branches for redefinition analysis ([#&#8203;12311](https://togithub.com/astral-sh/ruff/pull/12311))
-   \[`refurb`] Restrict forwarding for `newline` argument in `open()` calls to Python versions >= 3.10 ([#&#8203;12244](https://togithub.com/astral-sh/ruff/pull/12244))

##### Documentation

-   Update help and documentation to reflect `--output-format full` default ([#&#8203;12248](https://togithub.com/astral-sh/ruff/pull/12248))

##### Performance

-   Use more threads when discovering Python files ([#&#8203;12258](https://togithub.com/astral-sh/ruff/pull/12258))

### [`v0.5.1`](https://togithub.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#051)

[Compare Source](https://togithub.com/astral-sh/ruff/compare/0.5.0...0.5.1)

##### Preview features

-   \[`flake8-bugbear`] Implement mutable-contextvar-default (B039) ([#&#8203;12113](https://togithub.com/astral-sh/ruff/pull/12113))
-   \[`pycodestyle`] Whitespace after decorator (`E204`) ([#&#8203;12140](https://togithub.com/astral-sh/ruff/pull/12140))
-   \[`pytest`] Reverse `PT001` and `PT0023` defaults ([#&#8203;12106](https://togithub.com/astral-sh/ruff/pull/12106))

##### Rule changes

-   Enable token-based rules on source with syntax errors ([#&#8203;11950](https://togithub.com/astral-sh/ruff/pull/11950))
-   \[`flake8-bandit`] Detect `httpx` for `S113` ([#&#8203;12174](https://togithub.com/astral-sh/ruff/pull/12174))
-   \[`numpy`] Update `NPY201` to include exception deprecations ([#&#8203;12065](https://togithub.com/astral-sh/ruff/pull/12065))
-   \[`pylint`] Generate autofix for `duplicate-bases` (`PLE0241`) ([#&#8203;12105](https://togithub.com/astral-sh/ruff/pull/12105))

##### Server

-   Avoid syntax error notification for source code actions ([#&#8203;12148](https://togithub.com/astral-sh/ruff/pull/12148))
-   Consider the content of the new cells during notebook sync ([#&#8203;12203](https://togithub.com/astral-sh/ruff/pull/12203))
-   Fix replacement edit range computation ([#&#8203;12171](https://togithub.com/astral-sh/ruff/pull/12171))

##### Bug fixes

-   Disable auto-fix when source has syntax errors ([#&#8203;12134](https://togithub.com/astral-sh/ruff/pull/12134))
-   Fix cache key collisions for paths with separators ([#&#8203;12159](https://togithub.com/astral-sh/ruff/pull/12159))
-   Make `requires-python` inference robust to `==` ([#&#8203;12091](https://togithub.com/astral-sh/ruff/pull/12091))
-   Use char-wise width instead of `str`-width ([#&#8203;12135](https://togithub.com/astral-sh/ruff/pull/12135))
-   \[`pycodestyle`] Avoid `E275` if keyword followed by comma ([#&#8203;12136](https://togithub.com/astral-sh/ruff/pull/12136))
-   \[`pycodestyle`] Avoid `E275` if keyword is followed by a semicolon ([#&#8203;12095](https://togithub.com/astral-sh/ruff/pull/12095))
-   \[`pylint`] Skip [dummy variables](https://docs.astral.sh/ruff/settings/#lint_dummy-variable-rgx) for `PLR1704` ([#&#8203;12190](https://togithub.com/astral-sh/ruff/pull/12190))

##### Performance

-   Remove allocation in `parse_identifier` ([#&#8203;12103](https://togithub.com/astral-sh/ruff/pull/12103))
-   Use `CompactString` for `Identifier` AST node ([#&#8203;12101](https://togithub.com/astral-sh/ruff/pull/12101))

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

_Review requested from @AlexWaygood by @renovate[bot] on 2024-08-12 02:00_

---

_Label `internal` added by @renovate[bot] on 2024-08-12 02:00_

---

_Comment by @AlexWaygood on 2024-08-12 06:39_

We shouldn't have received this PR. The renovate config is meant to only be giving us updates for requirements.txt files inside `doc/`:

https://github.com/astral-sh/ruff/blob/3481e16cdf4ef53e79bbaeedaf087fa97eb23553/.github/renovate.json5#L20

The file being touched touched here is the output of a `uv pip compile` command, which renovate doesn't know how to do, so this isn't really a suitable target for a renovate PR. Not sure what's going on here.

---

_Comment by @AlexWaygood on 2024-08-13 16:28_

I see the issue. The [renovate docs](https://docs.renovatebot.com/modules/manager/#ignoring-files-that-match-the-default-filematch) say that setting `fileMatch` _extends_ renovate's default for a certain package manager rather than overwriting it. That's not at all what I would expect 😕 Renovate's default for `pip_requirements` is to search for `requirements.txt` files across the whole repository, so that's why it's finding this one.

---

_Renamed from "Update dependency ruff to v0.5.7" to "Update dependency ruff to v0.5.7 - autoclosed" by @renovate[bot] on 2024-08-13 17:16_

---

_Closed by @renovate[bot] on 2024-08-13 17:16_

---

_Branch deleted on 2024-08-13 17:16_

---
