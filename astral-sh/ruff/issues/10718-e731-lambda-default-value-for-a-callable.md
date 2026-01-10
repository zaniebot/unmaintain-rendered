---
number: 10718
title: "E731: Lambda default value for a Callable dataclass field is incorrectly flagged as lambda assignment"
type: issue
state: closed
author: richardxia
labels:
  - rule
assignees: []
created_at: 2024-04-01T17:18:55Z
updated_at: 2024-04-01T19:50:04Z
url: https://github.com/astral-sh/ruff/issues/10718
synced_at: 2026-01-10T01:22:50Z
---

# E731: Lambda default value for a Callable dataclass field is incorrectly flagged as lambda assignment

---

_Issue opened by @richardxia on 2024-04-01 17:18_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The pycodestyle rule [lambda-assignment (E731)](https://docs.astral.sh/ruff/rules/lambda-assignment/) is supposed to check whether a lambda expression is being assigned to a variable and instead suggest using a `def` statement. However, Ruff's implementation appears to be incorrectly flagging a dataclass field with a default value consisting of a lambda expression as violation of this rule, whereas pycodestyle does not. Here is a short example:

```python
from dataclasses import dataclass
from typing import Callable


@dataclass
class Filter:
    filter: Callable[[str], bool] = lambda _: True
```

Here is the command-line output of both Ruff and pycodestyle:

```sh
$ ruff check --isolated --select=E731 mycode.py 
mycode.py:7:5: E731 Do not assign a `lambda` expression, use a `def`
Found 1 error.
```

```sh
$ pycodestyle --select=E731 mycode.py
```

(No output for pycodestyle, but exits successfully with status code 0.)

The examples above were run with ruff 0.3.4 and pycodestyle 2.11.1, which are the latest versions at the time of this writing.

This should apply to more classes than just dataclasses, but I'm not exactly sure where I would draw the line. You probably do want to flag cases where someone assigned a lambda to a class variable when it should have been a class or instance method, but I'm not sure which heuristics you want to apply.

I haven't tried reading the pycodestyle source code to see how they implemented it, but playing with my example above, I think their heuristic may just be whether or not an annotation is present on the class member declaration, which seems reasonable to me. Here are a few specific example with inline comments on whether pycodestyle reports a violation:

```python
from dataclasses import dataclass
from typing import Callable


# pycodestyle is happy with this
@dataclass
class FilterDataclass:
    filter: Callable[[str], bool] = lambda _: True


# pycodestyle is happy with this
class FilterClassWithCallableAnno:
    filter: Callable[[str], bool] = lambda _: True


# pycodestyle is happy with this
class FilterClassWithAribtraryAnno:
    filter: Foo = lambda _: True


# pycodestyle is _not_ happy with this
# mycode.py:24:5: E731 do not assign a lambda expression, use a def
class FilterClassWithoutAnno:
    filter = lambda _: True
```

---

_Comment by @charliermarsh on 2024-04-01 17:22_

Ah yeah that's interesting. I think Pyflakes only flags unannotated assignments.


---

_Comment by @charliermarsh on 2024-04-01 17:23_

I could imagine always ignoring these when they're defined within class scopes. But perhaps best to start with dataclasses.

---

_Label `rule` added by @charliermarsh on 2024-04-01 17:23_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-01 17:44_

---

_Referenced in [astral-sh/ruff#10720](../../astral-sh/ruff/pulls/10720.md) on 2024-04-01 18:04_

---

_Closed by @charliermarsh on 2024-04-01 19:44_

---

_Comment by @richardxia on 2024-04-01 19:50_

Thanks for the super fast response and fix!

---

_Referenced in [astral-sh/ruff#13336](../../astral-sh/ruff/issues/13336.md) on 2024-09-12 19:45_

---
