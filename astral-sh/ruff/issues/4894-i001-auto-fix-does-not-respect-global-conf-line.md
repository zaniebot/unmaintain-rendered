```yaml
number: 4894
title: "I001: Auto-fix does not respect global conf `line-length`"
type: issue
state: closed
author: pespinel
labels:
  - question
assignees: []
created_at: 2023-06-06T07:01:33Z
updated_at: 2023-06-07T03:44:56Z
url: https://github.com/astral-sh/ruff/issues/4894
synced_at: 2026-01-12T15:54:45Z
```

# I001: Auto-fix does not respect global conf `line-length`

---

_@pespinel_

The I001 auto-fix does not respect the global line-length ruff config.

Example:
```
from acceptance.app.novum.pageelements.internalwebviewnavigationtopbar import AndroidInternalWebviewNavigationTopBar
from acceptance.app.novum.pageobjects.base.profile.security_and_privacy.change_security_question.\
    change_security_question import BaseChangeSecurityQuestion
```

After running ruff with auto-fix:

```
from acceptance.app.novum.pageelements.internalwebviewnavigationtopbar import AndroidInternalWebviewNavigationTopBar
from acceptance.app.novum.pageobjects.base.profile.security_and_privacy.change_security_question.change_security_question import (
    BaseChangeSecurityQuestion,
)
```
`E501 Line too long (130 > 120 characters)`

Ruff version: ` 0.0.270`

Ruff config: 

```
select = [
    #"A", # Flake8 builtins
    #"B", # Flake8 bugbear
    #"D", # Pydocstyle
    "E", # Pycodestyle errors
    "F", # Pyflakes
    "G", # Flake8 logging-format
    "I", # Isort
    "N", # Pep8-naming
    #"Q", # Flake8-quotes
    #"S", # Flake8 bandit
    "W", # Pycodestyle warnings
    "ANN", # Flake8 annotations
    "ASYNC", # Flake8 async
    "ARG", # Flake8 unused-arguments
    #"BLE", # Flake8 blind-except
    #"COM", # Flake8 commas
    #"C4", # Fake8 comprehensions
    "C90", # McCabe
    #"DTZ", # Flake8 datetime
    #"EM", # Flake8 errmsg
    #"EXE", # Flake8 executable
    #"ERA", # Eradicate
    "FA", # Flake8 future-annotations
    #"FBT", # Flake8 boolean-trap
    "FLY", # Flake 8 misc lints
    "INP", # Flake8 implicit-namespace
    "INT", # Flake8 gettext
    #"ISC", # Flake8 implicit-str-concat
    "ICN", # Flake8 import-conventions
    "NPY", # Numpy
    "PYI", # Flake8 pyi
    "PIE", # Flake8 pie
    #"PT", # Flake8 pytest
    #"PTH", # Flake8 path-lib
    "PL", # Pylint
    #"RET", # Flake8 return
    #"RSE", # Flake8 raise
    #"RUF", # Ruff
    #"SLF", # Flake8 self
    #"SIM", # Flake8 simmplify
    "TCH", # Flake8 type-checking
    #"TD", # Flake8 todos
    "TID", # Flake8 tidy-imports
    #"TRY", # Tryceratops
    "T10", # Flake8 debugger
    "T20", # Flake8 print
    "TCH", # Flake8 type-checking
    #"UP", # Flake8 pyupgrade
    "YTT" # Flake8 2020
]
ignore = [
    "ARG001", # unused-function-argument
    "ARG002", # unused-method-argument
    "ARG005", # unused-lambda-argument
    "FLY002", # static-join-to-f-string
    "G001", # logging-format-interpolation
    "PLC1901", # compare-to-empty-string
    "PLR5501", # collapsible-else-if
    "PLR2004", # magic-value-comparison
    "PLW2901", # redefined-loop-name
]
fixable = ["F", "I"]
unfixable = []
exclude = [
    "acceptance/resources/users_data",
    "acceptance/resources/web_driver",
    "acceptance/resources/web_driver_agent",
    "build_tools",
    "docs",
    ".idea",
    ".vscode",
    ".venv",
    "venv",
    "_output",
    ".ruff_cache"
]
line-length = 120
target-version = "py38"
format = "github"

[pylint]
max-branches = 15
max-args = 8

[mccabe]
max-complexity = 15

[flake8-unused-arguments]
ignore-variadic-names = true

[flake8-annotations]
ignore-fully-untyped = true
```

---

_Renamed from "I001 auto-fix does not respect global conf `line-length`" to "I001: Auto-fix does not respect global conf `line-length`" by @pespinel on 2023-06-06 10:10_

---

_Comment by @charliermarsh on 2023-06-06 14:27_

Hmm, I believe it does respect it -- what output are you expecting here? `from acceptance.app.novum.pageobjects.base.profile.security_and_privacy.change_security_question.change_security_question` alone exceeds the line length, so there's no way to break up that import to adhere to your setting.

---

_Label `question` added by @charliermarsh on 2023-06-06 14:27_

---

_Comment by @pespinel on 2023-06-06 15:09_

> Hmm, I believe it does respect it -- what output are you expecting here? `from acceptance.app.novum.pageobjects.base.profile.security_and_privacy.change_security_question.change_security_question` alone exceeds the line length, so there's no way to break up that import to adhere to your setting.

@charliermarsh Before applying the solution, the import didn't break the `Line Too Long` rule. Maybe it would be possible to preserve the line break when applying the auto-fix? 

Also if I manually fix the E501 issue:

```
from acceptance.app.novum.pageelements.internalwebviewnavigationtopbar import AndroidInternalWebviewNavigationTopBar
from acceptance.app.novum.pageobjects.base.profile.security_and_privacy.change_security_question. \
    change_security_question import (
        BaseChangeSecurityQuestion,
)
```
The I001 appears again: `I001 Import block is un-sorted or un-formatted`


---

_Comment by @charliermarsh on 2023-06-06 19:00_

Ahh, ok, I see what you're saying! Thanks for clarifying.

Candidly, I'm comfortable with our current behavior, and I think it's preferable to enforce the current formatting over changing it to accommodate this style of line-length workaround. isort and Black exhibit the same behavior. If you're already implementing custom formatting for those long imports, then I'd suggest explicitly ignoring the line-length error like:

```py
from acceptance.app.novum.pageobjects.base.profile.security_and_privacy.change_security_question.change_security_question (  # noqa: E501
    BaseChangeSecurityQuestion,
)
```

---

_Closed by @charliermarsh on 2023-06-06 19:00_

---

_Comment by @dhruvmanila on 2023-06-07 03:44_

I would also suggest to use relative imports at that level of nested packages. This could simplify the import block and IMHO, more readable as well. Another solution could be to expose the symbols using a `__init__.py` file in a package so that your top level imports are clean (this is my best efforts, I'm not sure how your project is structured):

```python
from acceptance.app.novum.pageobjects.base import BaseChangeSecurityQuestion
```

---
