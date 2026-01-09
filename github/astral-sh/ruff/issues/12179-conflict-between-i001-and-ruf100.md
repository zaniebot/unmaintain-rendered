---
number: 12179
title: "(🐞) conflict between `I001` and `RUF100`"
type: issue
state: open
author: KotlinIsland
labels:
  - isort
  - suppression
assignees: []
created_at: 2024-07-04T05:03:30Z
updated_at: 2024-07-07T23:51:39Z
url: https://github.com/astral-sh/ruff/issues/12179
synced_at: 2026-01-07T13:12:15-06:00
---

# (🐞) conflict between `I001` and `RUF100`

---

_Issue opened by @KotlinIsland on 2024-07-04 05:03_

```py
from pydantic import BaseModel, ConfigDict, TypeAdapter as _TypeAdapter, ValidationError  # noqa: TID251
```
```toml
[lint.flake8-tidy-imports.banned-api]
"pydantic.BaseModel".msg = "use `AmongusBaseModel` instead"
```

When I run `ruff check --fix` it deletes the `noqa: TID251` and complains about `TID251` on that line
When I run `ruff check` it complains about `I001`

---

_Comment by @MichaReiser on 2024-07-04 06:31_

I tried to reproduce the error but failed. Can you share your isort configuration?

What I tried:

source file:

```python
from pydantic import BaseModel, ConfigDict, TypeAdapter as _TypeAdapter, ValidationError  # noqa: TID251


print(BaseModel, ConfigDict, _TypeAdapter, ValidationError)
```

Configuration

```toml
[lint]
preview = true
select = ["ALL", "RUF100"]
ignore = ["T201", "D100", "CPY001"]

[lint.flake8-tidy-imports]
banned-api = { "pydantic.BaseModel" = { "msg"  =  "use `AmongusBaseModel` instead" } }
```

Run `ruff check --fix`

```shell
uv tool run ruff check test2.py --config 12179.toml --fix
```

Source after:

```python
from pydantic import BaseModel, ConfigDict, ValidationError  # noqa: TID251
from pydantic import TypeAdapter as _TypeAdapter

print(BaseModel, ConfigDict, _TypeAdapter, ValidationError)
```

Run `ruff check` again

```shell
uv tool run ruff check test2.py --config 12179.toml 
warning: `uv tool run` is experimental and may change without warning.
Resolved 1 package in 3ms
Installed 1 package in 3ms
 + ruff==0.5.0
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
All checks passed!
```

---

_Comment by @KotlinIsland on 2024-07-04 06:44_

Sorry, I was in a rush. I think it's likely: `lint.isort.combine-as-imports = true`

<details><summary>Full config</summary>
<p>

```toml
target-version = "py312"
preview = true
unsafe-fixes = true
extend-exclude = ["pw"]

[lint]
extend-select = ["ALL"]
extend-unfixable = [
  "T201", # print
  "F841", # unused-variable (annoying when it runs locally and removes the variables you just added)
  "RUF027", #  missing-f-string-syntax, can be intentional
]
ignore = [
  # rules that are disabled due to a bug (make sure you include the issue link):
  "E721", # unidiomatic typecheck (using pylint's one for now due to https://github.com/astral-sh/ruff/issues/6465)
  "N818", # error-suffix-on-exception-name (https://github.com/astral-sh/ruff/issues/5367)
  "UP040", # non-pep695-type-alias (https://github.com/python/mypy/issues/15238)

  # rules that are disabled because it was too much work to fix existing instances in our codebase.
  # TODO: enable them once they're fixed or when ruff gets baseline https://github.com/astral-sh/ruff/issues/1149:
  "A003", # builtin-attribute-shadowing
  "ERA001", # commented-out-code
  "RUF012", # mutable-class-default
  "FBT001", # boolean-type-hint-positional-argument
  "FBT002", # boolean-default-value-positional-argument
  "FBT003", # boolean-positional-value-in-call
  "N815", # mixed-case-variable-in-class-scope
  "N803", # invalid-argument-name
  "TD003", # missing-todo-link
  "DTZ001", # call-datetime-without-tzinfo
  "DTZ002", # call-datetime-today
  "DTZ005", # call-datetime-now-without-tzinfo
  "DTZ006", # call-datetime-fromtimestamp
  "DTZ007", # call-datetime-strptime-without-zone
  "DTZ011", # call-date-today
  "DTZ012", # call-date-fromtimestamp
  "BLE001", # blind-except
  "SLF001", # private-member-access
  "TRY301", # raise-within-try
  "PLR0917", # Too many potisional arguments

  # rules that are disabled because they're covered by another tool
  "F821", # undefined-name (basedmypy)
  "PLE1507", # invalid-envvar-value (basedmypy)
  "RET503", # implicit-return (basedmypy)
  "ANN", # flake8-annotations (basedmypy)
  "PLE0605", # Invalid format for `__all__`, must be `tuple` or `list` (basedmypy)
  "PGH003", # blanket-type-ignore (basedmypy, basedpyright)
  "B018", # useless-expression (basedpyright)
  "E112", # no-indented-block (covered by pretty much every linter because it's a syntax error)
  "ISC001", # single-line-implicit-string-concatenation (ruff format)
  "COM812", # missing-trailing-comma (ruff format)
  "W291", # trailing-whitespace (ruff format)
  "W293", # blank-line-with-whitespace (ruff format, also https://github.com/astral-sh/ruff/issues/10038)

  # rules that are disabled because we don't like them or some other permanent reason:
  "EM", # flake8-errmsg
  "FIX", # flake8-fixme
  "S", # lake8-bandit (security related rules, we have no external/untrusted users so these are mostly useless)
  "E501", # Doc line too long
  "PLR0913", # Too many arguments to function call
  "PLR0912", # Too many branches
  "PLR0915", # Too many statements
  "PLR0916", # Too many Boolean expressions
  "PLR2004", # Magic value used in comparison
  "PLR1722", # Use `sys.exit()` instead of `exit`
  "PLW2901", # `for` loop variable overwritten by assignment target
  "PLR0911", # Too many return statements
  "PLW0127", # Self-assignment of variable (used when redefining variables)
  "PLW0603", # Using the global statement is discouraged
  "PLW1514", # `open` in text mode without explicit `encoding` argument (we set this globally in REDACTED)
  "PLC0105", # `TypeVar` name does not reflect its covariance
  "PLC0414", # Import alias does not rename original package (used by mypy for explicit re-export)
  "PLC1901", # compare-to-empty-string
  "PLC2701", # import-private-name, the underscore is clear enough
  "PLR0904", # too-many-public-methods
  "PLR0914", # too-many-local-variables
  "FURB171", # single-item-membership-test (used for checks where more values will be added in the future)
  "D100", # Missing docstring in public module (ideally would be enabled, but would be too annoying for stuff like REDACTED)
  "D105", # Missing docstring in magic method
  "D107", # Missing docstring in __init__
  "D200", # One-line docstring should fit on one line with quotes
  "D202", # No blank lines allowed after function docstring
  "D203", # 1 blank line required before class docstring
  "D205", # 1 blank line required between summary line and description
  "D209", # Multi-line docstring closing quotes should be on a separate line
  "D212", # Multi-line docstring summary should start at the first line
  "D213", # Multi-line docstring summary should start at the second line
  "D400", # First line should end with a period
  "D401", # First line should be in imperative mood
  "D402", # First line should not be the function's "signature"
  "D403", # First word of the first line should be properly capitalized
  "D404", # First word of the docstring should not be `This`
  "D405", # Section name should be properly capitalized
  "D406", # Section name should end with a newline
  "D407", # Missing dashed underline after section
  "D412", # No blank lines allowed between a section header and its content (required for pycharm to format example code correctly)
  "D413", # Missing blank line after last section
  "D415", # First line should end with a period, question mark, or exclamation point
  "D418", # Function/ Method decorated with @overload shouldn't contain a docstring (vscode supports it)
  "RUF001", # ambiguous-unicode-character-string (plenty of intentional instances when checking error messages etc)
  "SIM300", # yoda-conditions
  "C901", # complex-structure
  "CPY001", # missing-copyright-notice
  "TRY003", # raise-vanilla-args
  "S101", # assert (pytest uses assert statements)
  "TD002", # missing-todo-author
  "PT013", # pytest-incorrect-pytest-import
  "TRY002", # raise-vanilla-class
  "PERF401", # manual-list-comprehension (it suggests nested comprehensions which are unreadable)
  "SLOT000", # no-slots-in-str-subclass
  "PTH207", # glob (seems to require part path and part glob)
  "RET505", # superfluous-else-return (can make code harder to read)
  "TRY300", # try-consider-else (just a suggestion, sometimes makes sense, other times makes code harder to understand)
  "PLR1702", # too-many-nested-blocks
  "SIM118", # in-dict-keys (purely heuristic, https://github.com/astral-sh/ruff/issues/3961)
  "RUF018", # Avoid assignment expressions in `assert` statements (we never run python with asserts disabled)
]

[lint.per-file-ignores]
"*.pyi" = [
  "PLW1641", # Object does not implement `__hash__` method
  "TID252", # relative-imports
  "FURB180", # meta-class-abc-meta (conflicts with how mypy handles abstract classes in stubs)
  # we don't control names in 3rd party modules:
  "N", # pep8-naming
  "F811", # redefined-while-unused
  "PLW3201", # bad-dunder-method-name
  "A001", # builtin-variable-shadowing
  "A002", # builtin-argument-shadowing
  "N804", # invalid-first-argument-name-for-class-method
  "PYI042", # snake-case-type-alias
  "UP042", # replace-str-enum (we don't control what type of enums are used in stubs)
]
"scripts/**/*.py" = [
  "T201", # print (allowed in scripts but not in REDACTED)
  "T203", # pprint (allowed in scripts but not in REDACTED)
  "PT", # flake8-pytest-style (scripts are not executed by pytest)
]
"REDACTED/**/*.py" = [
  "T201", # print (allowed in sandpit files)
  "T203", # pprint (allowed in sandpit files)
]
"REDACTED/**/*.py" = [
  "PT", # flake8-pytest-style (utilities aren't always executed by pytest)
]
"REDACTED/**/*.py" = [
  "N999", # invalid-module-name (robot listener modules should have the same name as the class)
]
"REDACTED/**/*.py" = [
  "D101", # Missing docstring in public class
  "D102", # Missing docstring in public method
  "D103", # Missing docstring in public function
  "D104", # Missing docstring in public package
  "D106", # Missing docstring in nested class
]
"conftest.py" = [
  "D101", # Missing docstring in public class
  "D102", # Missing docstring in public method
  "D103", # Missing docstring in public function
  "D106", # Missing docstring in nested class
]
[lint.isort]
# ruff detects unrelated folder names as first party source code
known-third-party = ["coverage", "robot"]
combine-as-imports = true
split-on-trailing-comma = false

[lint.flake8-bugbear]
extend-immutable-calls = [
  "datetime.date.today", # very unlikely for tests to run across midnight
]

[lint.flake8-tidy-imports.banned-api]
"typing.NoReturn".msg = "use `typing.Never` instead" # https://github.com/astral-sh/ruff/issues/5241
"typing_extensions.deprecated".msg = "REDACTED" # https://youtrack.jetbrains.com/issue/PY-61651
"pytest_robotframework.as_keyword".msg = "REDACTED"
"unittest.TestCase".msg = "use pytest instead"
"pydantic.BaseModel".msg = "REDACTED"
"pydantic.validate_call".msg = "REDACTED"
"basedtyping.Function".msg = "REDACTED"
"basedtyping.Fn".msg = "REDACTED"
"types.FunctionType".msg = "REDACTED"
"pytest.mark".msg = "REDACTED"

[lint.pylint]
allow-dunder-method-names = ["__get_pydantic_core_schema__"]

[lint.flake8-type-checking]
runtime-evaluated-base-classes = [
  "pydantic.BaseModel",
  "REDACTED",
  "REDACTED",
]
runtime-evaluated-decorators = [
  "pydantic.validate_call",
  "pydantic.validate_call_decorator.validate_call",
  "pydantic.dataclasses.dataclass",
  "REDACTED",
  "REDACTED",
  "REDACTED",
  "REDACTED",
  "REDACTED",
]

[lint.flake8-pytest-style]
# This is unfortunate, but `pytest.fixture` contains `Any`, while `pytest.fixture()` doesn't
fixture-parentheses = true

[lint.pydocstyle]
ignore-decorators = ["functools.wraps"]

[format]
skip-magic-trailing-comma = true
```

</p>
</details> 


---

_Comment by @MichaReiser on 2024-07-04 08:46_

Thanks. Yes, I can reproduce with 

```toml

[lint]
preview = true
select = ["ALL", "RUF100"]
ignore = ["T201", "D100", "CPY001"]

[lint.flake8-tidy-imports]
banned-api = { "pydantic.BaseModel" = { "msg"  =  "use `AmongusBaseModel` instead" } }

[lint.isort]
# ruff detects unrelated folder names as first party source code
known-third-party = ["coverage", "robot"]
combine-as-imports = true
split-on-trailing-comma = false
```

If I run `--fix` without `RUF100`, then the result is

```python
from pydantic import (  # noqa: TID251
    BaseModel,
    ConfigDict,
    TypeAdapter as _TypeAdapter,
    ValidationError,
)

print(BaseModel, ConfigDict, _TypeAdapter, ValidationError)
```

The problem here is that the suppression comment gets moved after the opening `(`, which now no longer suppresses any violation. 

I don't think there's a good way to avoid this kind of problem except if Ruff would track to which node a suppression comment belongs.

---

_Label `suppression` added by @dhruvmanila on 2024-07-05 02:11_

---

_Comment by @KotlinIsland on 2024-07-05 07:22_

come to think of it, why is the presence of this pragma (`noqa`) causing a reformat? the line length is still 88

> I don't think there's a good way to avoid this kind of problem except if Ruff would track to which node a suppression comment belongs.

If that is too difficult, could this contradiction be detected, such that, after all the transformations are completed, the addition of a new error is detected, and no changes are made. instead an error is reported like "conflicting situation blah blah blah"

---

_Label `isort` added by @MichaReiser on 2024-07-05 08:05_

---

_Comment by @MichaReiser on 2024-07-05 08:07_

> come to think of it, why is the presence of this pragma (noqa) causing a reformat? the line length is still 88

`from pydantic import BaseModel, ConfigDict, TypeAdapter as _TypeAdapter, ValidationError  # noqa: TID251` has a length of 105 or 91 excluding the comment, both exceeding the max line length of 88. 

So I think this really isn't about it being  conflict but just that a suppression comment might get moved to a place where it no longer suppresses anything.

---

_Comment by @KotlinIsland on 2024-07-07 23:20_

`from pydantic import BaseModel, ConfigDict, TypeAdapter as _TypeAdapter, ValidationError` is 88 chars, so it doesn't trigger `I001`, but with the prgama included it is 105

should `noqa` count towards the line length? if it didn't it would fix this specific issue


I think a solid solution would be:
```py
from pydantic import BaseModel, ConfigDict, TypeAdapter as _TypeAdapter, ValidationError  # noqa: TID251
```
```
> ruff check --fix
test.py:1: applying fix for `I001` would result in new errors `RUF100` + `TID251` and fixing those would result in the fix for `I0001` needing to be reverted. So what we are going to do is fix `I001`, but leave the `RUF100` and the `TID251`, and you can sort it out from there.
```

---

_Referenced in [astral-sh/ruff#12901](../../astral-sh/ruff/issues/12901.md) on 2024-09-12 23:42_

---

_Referenced in [astral-sh/ruff#18791](../../astral-sh/ruff/issues/18791.md) on 2025-06-19 12:34_

---

_Referenced in [astral-sh/ruff#20042](../../astral-sh/ruff/issues/20042.md) on 2025-08-22 14:11_

---
