```yaml
number: 12172
title: "`PLR6301` doesn't consider base class methods"
type: issue
state: open
author: minusf
labels:
  - bug
  - type-inference
assignees: []
created_at: 2024-07-03T15:51:07Z
updated_at: 2025-08-11T22:21:10Z
url: https://github.com/astral-sh/ruff/issues/12172
synced_at: 2026-01-12T15:54:51Z
```

# `PLR6301` doesn't consider base class methods

---

_@minusf_

Consider the following simple example (and common python pattern in general) in django:
to build a management command, one simply inherits from `BaseCommand` and overrides some methods:

```py
from django.core.management.base import BaseCommand, CommandError
class Command(BaseCommand):
    def add_arguments(self, parser):
        pass
    def handle(self, *args, **options):
        pass
```

Currently both of these are marked by ruff 0.5.0 as PLR6301 (https://docs.astral.sh/ruff/rules/no-self-use/).

I tell a lie: I copy pasted this "minimal" example into a new file and there is no warning 😁 
However the moment I add actual code to the methods, the warnings show up:

```py
from django.core.management.base import BaseCommand, CommandError
class Command(BaseCommand):
    def add_arguments(self, parser): 
        parser.add_argument("--id", required=True, type=int, help="Page id")

    def handle(self, *args, **options):
        pass
```

Out of curiosity I installed `pylint` and ran it on the file and it does not trigger this message...

```
[tool.ruff]
line-length = 100

[tool.ruff.lint]
preview = true
select = [
    "A",      # https://pypi.org/project/flake8-builtins/
    "B",      # https://pypi.org/project/flake8-bugbear/
    "C4",     # https://pypi.org/project/flake8-comprehensions/
    "C90",    # https://pypi.org/project/mccabe/
    "DJ",     # https://pypi.org/project/flake8-django/
    "DTZ",    # https://pypi.org/project/flake8-datetimez/
    "E", "W", # https://pypi.org/project/pycodestyle/
    "F",      # https://pypi.org/project/pyflakes/
    "FURB",   # https://pypi.org/project/refurb/
    "G",      # https://pypi.org/project/flake8-logging-format/
    "I",      # https://pypi.org/project/isort/
    "N",      # https://pypi.org/project/pep8-naming/
    "PERF",   # https://pypi.org/project/perflint/
    "PIE",    # https://pypi.org/project/flake8-pie/
    "PL",     # https://pypi.org/project/pylint/
    "PT",     # https://pypi.org/project/flake8-pytest-style/
    "PTH",    # https://pypi.org/project/flake8-use-pathlib/
    "RET",    # https://pypi.org/project/flake8-return/
    "RUF",    # Ruff-specific rules
    "S",      # https://pypi.org/project/flake8-bandit/
    "SIM",    # https://pypi.org/project/flake8-simplify/
    "UP",     # https://pypi.org/project/pyupgrade/
]
ignore = ["RUF012"]

[tool.ruff.lint.isort]
# Use a single line between direct and from import.
lines-between-types = 1
section-order = ["future", "standard-library", "third-party", "django", "wagtail", "first-party", "local-folder"]

[tool.ruff.lint.isort.sections]
# Group all Django imports into a separate section.
"django" = ["django"]
"wagtail" = ["wagtail"]
```

---

_Comment by @dhruvmanila on 2024-07-04 04:51_

I think this is because we're missing a check for method overrides from the base class which is done by Pylint (https://github.com/pylint-dev/pylint/blob/c8351395008e586170b0dda680594ba29b8fe0ef/pylint/extensions/no_self_use.py#L90).

We could possibly add this check with the current semantic model but it will only work if the base class is in the same file. In your case, it's being imported from another module which will require multifile analysis.

---

_Label `bug` added by @dhruvmanila on 2024-07-04 04:51_

---

_Label `multifile-analysis` added by @dhruvmanila on 2024-07-04 04:51_

---

_Renamed from "PLR6301 false positives" to "`PLR6301` doesn't consider base class methods" by @dhruvmanila on 2024-07-04 04:52_

---

_Comment by @minusf on 2024-07-04 08:01_

Thank you for the quick analysis. This rule is clearly in preview mode, but in my understanding until multifile analysis is being done, even simple cases like this will be false positives. What is ruff's rule policy for these cases?

---

_Comment by @charliermarsh on 2024-07-04 12:22_

The general suggestion for now is to mark such methods as overrides: https://play.ruff.rs/d0b3d8f4-1706-4a22-b19f-ea28c7acaacc

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---

_Comment by @DaniBodor on 2025-01-15 12:13_

I ran into something similar with a class that immediately raises a `NotImplementedError`. 
I've previously raised a similar issue (#12427) and there it was fixed using some generic definition of a "base class stub". I wonder whether that same definition could be used to determine when to flag PLR6301 and when not to.

---

_Comment by @spaceone on 2025-08-11 22:21_

I have the same issue with a different library.
I would like to just add a `# noqa: PLR6301` to the whole class:

```python3
class Command(BaseCommand):  # noqa: PLR6301
    def add_arguments(self, parser): 
        print(1)

    def handle(self, *args, **options):
        print(2)
```

---
