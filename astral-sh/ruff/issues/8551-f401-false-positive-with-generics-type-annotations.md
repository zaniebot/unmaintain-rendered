```yaml
number: 8551
title: F401 False Positive with Generics Type Annotations
type: issue
state: closed
author: Kaelten
labels:
  - bug
  - question
assignees: []
created_at: 2023-11-07T23:44:13Z
updated_at: 2023-11-10T05:06:51Z
url: https://github.com/astral-sh/ruff/issues/8551
synced_at: 2026-01-10T11:09:50Z
```

# F401 False Positive with Generics Type Annotations

---

_Issue opened by @Kaelten on 2023-11-07 23:44_

I have a generic base class in my code base that, to prevent circular imports, must have a forward reference used in it's type annotation.

Unfortunately, the type checking block keeps getting flagged as in violation of F401.
I've reconstructed a very arbitrary example below. 

**Ruff Version:** 0.1.3
**Command:** Not 100% sure because it's being ran from the pycharm integration when this happens.

**Config**

```toml
[tool.ruff]
output-format = "grouped"
target-version = "py311"

line-length = 120

# enable everything by default, and opt out as needed
select = ["ALL"]
ignore = [
    # turning off a few settings to make ruff linter warnings happy
    "ISC001", # flake8-implicit-str-concat
    # flake8-annotations
    "ANN101", # missing-type-self
    "ANN102", # missing-type-cls
    "ANN401", # any-type
    # flake8-bandit
    "S603", # subprocess-without-shell-equals-true
    # flake8-builtins
    "A003", # builtin-attribute-shadowing
    # flake8-fixme
    "FIX002", # line-contains-todo
    # flake8-todos
    "TD003", # missing-todo-link
    # pydocstyle
    "D100", # undocumented-public-module
    "D102", # undocumented-public-method
    "D104", # undocumented-public-package
    "D105", # undocumented-magic-method
    "D106", # undocumented-public-nested-class
    "D107", # undocumented-public-init
    "D203", # one-blank-line-before-class, conflicts with D211
    "D212", # multi-line-summary-first-line, conflicts with D213
    "D215", # section-underline-not-over-indented
    "D406", # new-line-after-section-name
    "D407", # dashed-underline-after-section
    "D408", # section-underline-after-name
    "D409", # section-underline-matches-section-length
    "D413", # blank-line-after-last-section
    # pylint
    "PLC0105", # type-name-incorrect-variance
]

# the below rules can fight in progress development and as such we won't autofix them
unfixable = [
    # eradicate
    "ERA001", # commented-out-code
    # flake8-bugbear
    "B007", # unused-loop-control-variable
    # perflint
    "PERF102", # incorrect-dict-iterator
    # pyflakes
    "F841", # unused-variable
]

[tool.ruff.format]
quote-style = "single"

[tool.ruff.flake8-bugbear]
extend-immutable-calls = [
    "fastapi.Depends",
    "fastapi.Form",
    "fastapi.Header",
    "fastapi.Query",
]

[tool.ruff.flake8-quotes]
inline-quotes = "single"

[tool.ruff.flake8-tidy-imports]
ban-relative-imports = "all"

[tool.ruff.flake8-tidy-imports.banned-api]
"typing.TypedDict".msg = "Pydantic requires 3.12 backports for TypedDict validations, use `typing_extensions.TypedDict` instead."

[tool.ruff.isort]
combine-as-imports = true
force-wrap-aliases = true
lines-between-types = 1

section-order = [
    "future",
    "standard-library",
    "third-party",
    "shared",
    "services",
    "tools",
    "first-party",
    "local-folder",
]

[tool.ruff.isort.sections]
"services" = [
    "serviceName1",
]

"shared" = [
    "sharedPackage",
]

"tools" = [
    "toolPackage",
]

[tool.ruff.pep8-naming]
# without this setting these decorators would trigger N805 false positives
classmethod-decorators = [
    "pydantic.field_validator",
    "pydantic.model_validator",
    "sqlalchemy.orm.declared_attr",
    "sqlalchemy.orm.declared_attr.directive",
]
```


**Reproduction case:**

```python
from typing import TYPE_CHECKING, Generic, TypeVar

if TYPE_CHECKING:
    from typing import List  # noqa: F401

_T = TypeVar('_T')


class MyGeneric(Generic[_T]):
    """My Generic Base."""


class Test(MyGeneric['List[int]']):
    """My non generic thing."""

```

---

_Label `bug` added by @charliermarsh on 2023-11-09 02:03_

---

_Comment by @charliermarsh on 2023-11-09 03:24_

I don't believe we support arbitrary generics -- can you try adding it to [`extend-generics`](https://docs.astral.sh/ruff/settings/#pyflakes-extend-generics)?

---

_Label `question` added by @charliermarsh on 2023-11-09 03:26_

---

_Comment by @Kaelten on 2023-11-09 17:27_

That was it!  I'd think it'd be possible, in some situations at least, to recognize arbitrary generics and the associated type annotations.  Is it something you plan to investigate further?

---

_Comment by @charliermarsh on 2023-11-10 05:06_

I think we will eventually! But I'm gonna mark it as out of scope for the issue for now.

---

_Closed by @charliermarsh on 2023-11-10 05:06_

---
