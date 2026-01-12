```yaml
number: 10058
title: "(🐞) Cache moment \"error: Failed to create fix for MetaClassABCMeta: Unable to insert `ABC` into scope due to name conflict\" failure"
type: issue
state: closed
author: KotlinIsland
labels:
  - bug
assignees: []
created_at: 2024-02-20T06:12:44Z
updated_at: 2025-01-27T01:27:55Z
url: https://github.com/astral-sh/ruff/issues/10058
synced_at: 2026-01-12T15:54:49Z
```

# (🐞) Cache moment "error: Failed to create fix for MetaClassABCMeta: Unable to insert `ABC` into scope due to name conflict" failure

---

_@KotlinIsland_

ruff 0.2.2
Windows 10/11

![image](https://github.com/astral-sh/ruff/assets/65446343/665fc2f6-9745-4113-9db3-d11e1bde6798)

```
> ruff check
error: Failed to create fix for MetaClassABCMeta: Unable to insert `ABC` into scope due to name conflict
```
Ruff returns exit code 0

This happens intermittently and I haven't been able to narrow down what is causing it.

<details><summary>config file:</summary>
<p>


```toml
[tool.ruff]
target-version = "py312"
preview = true
unsafe-fixes = true
extend-exclude = ["pw"]

[tool.ruff.lint]
extend-select = ["ALL"]
ignore = [
    # rules that are disabled due to a bug (make sure you include the issue link):
    "E112",  # no-indented-block (https://github.com/astral-sh/ruff/issues/6931 and https://github.com/astral-sh/ruff/issues/6930)
    "E721",  # unidiomatic typecheck (using pylint's one for now due to https://github.com/astral-sh/ruff/issues/6465)
    "N818",  # error-suffix-on-exception-name (https://github.com/astral-sh/ruff/issues/5367)
    "UP040", # non-pep695-type-alias (https://github.com/python/mypy/issues/15238)

    # rules that are disabled because it was too much work to fix existing instances in our codebase.
    # TODO: enable them once they're fixed or when ruff gets baseline https://github.com/astral-sh/ruff/issues/1149:
    "A003",    # builtin-attribute-shadowing
    "ERA001",  # commented-out-code
    "RUF012",  # mutable-class-default
    "FBT001",  # boolean-type-hint-positional-argument
    "FBT002",  # boolean-default-value-positional-argument
    "FBT003",  # boolean-positional-value-in-call
    "N815",    # mixed-case-variable-in-class-scope
    "N803",    # invalid-argument-name
    "TD003",   # missing-todo-link
    "DTZ001",  # call-datetime-without-tzinfo
    "DTZ002",  # call-datetime-today
    "DTZ005",  # call-datetime-now-without-tzinfo
    "DTZ006",  # call-datetime-fromtimestamp
    "DTZ007",  # call-datetime-strptime-without-zone
    "DTZ011",  # call-date-today
    "DTZ012",  # call-date-fromtimestamp
    "BLE001",  # blind-except
    "SLF001",  # private-member-access
    "TRY301",  # raise-within-try
    "PLR0917", # Too many potisional arguments

    # rules that are disabled because we don't like them or some other permanent reason:
    "ANN",     # flake8-annotations (covered by mypy)
    "EM",      # flake8-errmsg
    "FIX",     # flake8-fixme
    "S",       # lake8-bandit (security related rules, we have no external/untrusted users so these are mostly useless)
    "E501",    # Doc line too long
    "PLR0913", # Too many arguments to function call
    "PLR0912", # Too many branches
    "PLR0915", # Too many statements
    "PLR0916", # Too many Boolean expressions
    "PLR2004", # Magic value used in comparison
    "PLR1722", # Use `sys.exit()` instead of `exit`
    "PLW2901", # `for` loop variable overwritten by assignment target
    "PLE0605", # Invalid format for `__all__`, must be `tuple` or `list` (covered by mypy)
    "PLR0911", # Too many return statements
    "PLW0603", # Using the global statement is discouraged
    "PLW1514", # `open` in text mode without explicit `encoding` argument (we set this globally in the setup script)
    "PLC0105", # `TypeVar` name does not reflect its covariance
    "PLC0414", # Import alias does not rename original package (used by mypy for explicit re-export)
    "PLC1901", # compare-to-empty-string
    "PLC2701", # import-private-name, the underscore is clear enough
    "PLR0904", # too-many-public-methods
    "PLR0914", # too-many-local-variables
    "FURB171", # single-item-membership-test (used for checks where more values will be added in the future)
    "D100",    # Missing docstring in public module (ideally would be enabled, but would be too annoying for stuff like REDACTED where you'd have to add a useless docstring to each file)
    "D105",    # Missing docstring in magic method
    "D107",    # Missing docstring in __init__
    "D200",    # One-line docstring should fit on one line with quotes
    "D202",    # No blank lines allowed after function docstring
    "D203",    # 1 blank line required before class docstring
    "D205",    # 1 blank line required between summary line and description
    "D209",    # Multi-line docstring closing quotes should be on a separate line
    "D212",    # Multi-line docstring summary should start at the first line
    "D213",    # Multi-line docstring summary should start at the second line
    "D400",    # First line should end with a period
    "D401",    # First line should be in imperative mood
    "D402",    # First line should not be the function's "signature"
    "D403",    # First word of the first line should be properly capitalized
    "D404",    # First word of the docstring should not be `This`
    "D405",    # Section name should be properly capitalized
    "D406",    # Section name should end with a newline
    "D407",    # Missing dashed underline after section
    "D412",    # No blank lines allowed between a section header and its content (required for pycharm to format example code correctly)
    "D413",    # Missing blank line after last section
    "D415",    # First line should end with a period, question mark, or exclamation point
    "D418",    # Function/ Method decorated with @overload shouldn't contain a docstring (vscode supports it)
    "RUF001",  # ambiguous-unicode-character-string (plenty of intentional instances when checking error messages etc)
    "SIM300",  # yoda-conditions
    "RET503",  # implicit-return (covered by mypy)
    "C901",    # complex-structure
    "CPY001",  # missing-copyright-notice
    "TRY003",  # raise-vanilla-args
    "S101",    # assert (pytest uses assert statements)
    "TD002",   # missing-todo-author
    "PT013",   # pytest-incorrect-pytest-import
    "TRY002",  # raise-vanilla-class
    "PERF401", # manual-list-comprehension (it suggests nested comprehensions which are unreadable)
    "SLOT000", # no-slots-in-str-subclass
    "PTH207",  # glob (seems to require part path and part glob)
    "RET505",  # superfluous-else-return (can make code harder to read)
    "PGH003",  # blanket-type-ignore (covered by mypy)
    "TRY300",  # try-consider-else (just a suggestion, sometimes makes sense, other times makes code harder to understand)
    "PLR1702", # too-many-nested-blocks
    "ISC001",  # single-line-implicit-string-concatenation (conflicts with ruff formatter)
    "COM812",  # missing-trailing-comma (conflicts with ruff formatter)
    "W293",    # blank-line-with-whitespace (handled by ruff formatter, also https://github.com/astral-sh/ruff/issues/10038)
    "E301",    # blank-line-between-methods (conflicts with ruff formatter, https://github.com/astral-sh/ruff/issues/10039)
    "E302",    # blank-lines-top-level (conflicts with ruff formatter, https://github.com/astral-sh/ruff/issues/10039)
    "E305",    # blank-lines-after-function-or-class (conflicts with ruff formatter, https://github.com/astral-sh/ruff/issues/10039)
]

[tool.ruff.lint.per-file-ignores]
"*.pyi" = [
    "PLW1641", # Object does not implement `__hash__` method
    "TID252",  # relative-imports
    "FURB180", # meta-class-abc-meta (conflicts with how mypy handles abstract classes in stubs)
    # we don't control names in 3rd party modules:
    "N",       # pep8-naming
    "F811",    # redefined-while-unused
    "PLW3201", # bad-dunder-method-name
    "A001",    # builtin-variable-shadowing
    "A002",    # builtin-argument-shadowing
    "N804",    # invalid-first-argument-name-for-class-method
    "PYI042",  # snake-case-type-alias
]
"scripts/**/*.py" = [
    "T201", # print (allowed in scripts but not in REDACTED because you should use robot logging instead)
    "T203", # pprint (allowed in scripts but not in REDACTED because you should use robot logging instead)
    "PT",   # flake8-pytest-style (scripts are not executed by pytest)
]
"REDACTED/**/*.py" = [
    "T201", # print (allowed in REDACTED files)
    "T203", # pprint (allowed in REDACTED files)
]
"REDACTED/**/*.py" = [
    "PT", # flake8-pytest-style (REDACTED aren't always executed by pytest)
]
"REDACTED/**/*.py" = [
    "N999", # invalid-module-name (robot listener modules should have the same name as the class)
]
"tests/unit/**/*.py" = [
    "D101", # Missing docstring in public class
    "D102", # Missing docstring in public method
    "D103", # Missing docstring in public function
    "D104", # Missing docstring in public package
]
"conftest.py" = [
    "D101", # Missing docstring in public class
    "D102", # Missing docstring in public method
    "D103", # Missing docstring in public function
]
[tool.ruff.lint.isort]
# ruff detects unrelated folder names as first party source code
known-third-party = ["coverage", "robot"]
combine-as-imports = true
split-on-trailing-comma = false

[tool.ruff.lint.flake8-bugbear]
extend-immutable-calls = [
    "datetime.date.today", # very unlikely for tests to run across midnight
]

[tool.ruff.lint.flake8-tidy-imports.banned-api]
"typing.NoReturn".msg = "use `typing.Never` instead"                                             # https://github.com/astral-sh/ruff/issues/5241
"typing_extensions.deprecated".msg = "use `REDACTED` instead"                 # https://youtrack.jetbrains.com/issue/PY-61651
"pytest_robotframework.as_keyword".msg = "use `REDACTED` instead"
"unittest.TestCase".msg = "use pytest instead"

[tool.ruff.lint.pylint]
allow-dunder-method-names = ["__get_pydantic_core_schema__"]

[tool.ruff.lint.flake8-type-checking]
runtime-evaluated-base-classes = ["pydantic.BaseModel", "REDACTED"]
runtime-evaluated-decorators = [
    "pydantic.validate_call",
    "pydantic.dataclasses.dataclass",
    "REDACTED",
    "REDACTED",
    "REDACTED",
    "REDACTED"
]

[tool.ruff.lint.flake8-pytest-style]
# This is unfortunate, but `pytest.fixture` contains `Any`, while `pytest.fixture()` doesn't
fixture-parentheses = true

[tool.ruff.format]
skip-magic-trailing-comma = true
```

</p>
</details> 


---

_Comment by @charliermarsh on 2024-02-20 06:14_

This typically means that the file contains some symbol named `ABC`, so we can't add `from abc import ABC` to the file, and thus can't generate the fix.

---

_Comment by @KotlinIsland on 2024-02-20 06:15_

Why does ruff not fail then?

---

_Comment by @charliermarsh on 2024-02-20 06:17_

Sorry, what do you mean? It's a failure to create the fix, so the fix just won't be offered for that violation. (Arguably we should not show these warnings to users.)

---

_Comment by @charliermarsh on 2024-02-20 06:18_

Like, the net effect here is that you see a violation that's missing an autofix that would otherwise be available.

---

_Comment by @KotlinIsland on 2024-02-20 06:18_

![image](https://github.com/astral-sh/ruff/assets/65446343/74df3bc1-0b0b-4ccb-9a25-494ac0598886)


---

_Comment by @KotlinIsland on 2024-02-20 06:19_

the 19 errors went away because I changed the config, but that command didn't fail, and did show an error

---

_Comment by @charliermarsh on 2024-02-20 06:22_

These are more "warnings" than "errors", they won't cause `ruff check` to fail. But it _should_ be showing you lint errors for the relevant rule.

---

_Comment by @charliermarsh on 2024-02-20 16:26_

Are you able to share the file, or a subset of the file that reproduces it?

---

_Label `needs-info` added by @charliermarsh on 2024-02-20 16:26_

---

_Comment by @KotlinIsland on 2024-02-21 22:46_

At this point, I've only seen it when checking the entire code base, and I can't bisect it down because I don't have a consistent repro. Additionally, I can't share the codebase 😭

---

_Comment by @KotlinIsland on 2024-04-02 06:49_

# Caught it!
It only appears when the cache isn't used:
```py
from abc import ABCMeta


class ABC(metaclass=ABCMeta):  # noqa: FURB180
    ...
```
```toml
[tool.ruff]
unsafe-fixes = true

[tool.ruff.lint]
select = ["FURB180"]
preview = true
```
```
> ruff check test.py
error: Failed to create fix for MetaClassABCMeta: Unable to insert `ABC` into scope due to name conflict
All checks passed!
```

---

_Renamed from "(🐞) Mysterious "error: Failed to create fix for MetaClassABCMeta: Unable to insert `ABC` into scope due to name conflict" failure" to "(🐞) Cache moment "error: Failed to create fix for MetaClassABCMeta: Unable to insert `ABC` into scope due to name conflict" failure" by @KotlinIsland on 2024-04-02 07:11_

---

_Comment by @MichaReiser on 2024-04-02 09:29_

> It only appears when the cache isn't used:

This is because Ruff tries to create the fix on the first run (and fails) but then caches the diagnostic without the fix (because it is unlikely to succeed when doing so again). 

Nice narrowing of the error into a minimal repro. I can reproduce with the given configuration, test code, and the latest ruff version.

What I consider a bug here is that Ruff emits the error message even when the diagnostic is suppressed. There's no way for users to avoid the error message (other than fixing the code manually). Suppressing the rule isn't sufficient because Ruff first creates all diagnostics (including fixes) and only then filters out suppressed diagnostics. Fixing this would require (and I think that would be a good change overall, especially for the editor) splitting rules into a diagnostic generation and fix generation phase, and Ruff would only pull the fixes for not suppressed diagnostics.

---

_Label `needs-info` removed by @MichaReiser on 2024-04-02 09:29_

---

_Label `bug` added by @MichaReiser on 2024-04-02 09:29_

---

_Comment by @charliermarsh on 2025-01-27 01:27_

I believe this is no longer shown by default (only under `--verbose`), which seems sufficient to me.

---

_Closed by @charliermarsh on 2025-01-27 01:27_

---
