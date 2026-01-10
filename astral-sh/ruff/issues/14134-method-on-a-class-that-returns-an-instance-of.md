```yaml
number: 14134
title: method on a class that returns an instance of that class gets F821 (undefined name) for the annotated return type
type: issue
state: closed
author: JoranDox
labels:
  - question
  - needs-mre
assignees: []
created_at: 2024-11-06T13:18:47Z
updated_at: 2024-11-06T13:47:20Z
url: https://github.com/astral-sh/ruff/issues/14134
synced_at: 2026-01-10T11:09:55Z
```

# method on a class that returns an instance of that class gets F821 (undefined name) for the annotated return type

---

_Issue opened by @JoranDox on 2024-11-06 13:18_

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
file `path/to/myclass.py` contents:
```
class Myclass:
    def returnself(self) -> Myclass:
        return self
```
command: `ruff check --isolated path/to/myclass.py`
error:
```
path/to/myclass.py:2:29: F821 Undefined name `Myclass`
  |
1 | class Myclass:
2 |     def returnself(self) -> Myclass:
  |                             ^^^^^^^ F821
3 |         return self
  |

Found 1 error.
```
This code (in a much more complex form, specifically as a factory method for a class) works fine in production code, and I see no reason the linter shouldn't just see the name from right above.

ruff version: ruff 0.5.4

keywords searched for: F821, return type, classmethod, factory method

---

_Comment by @MichaReiser on 2024-11-06 13:21_

I believe the above syntax is only valid when importing `annotations` from `__future__` (https://peps.python.org/pep-0563/). See https://play.ruff.rs/89d2b517-496d-473d-8208-25afdf2ac177

But I let @carljm or @AlexWaygood weigh in on this

---

_Label `question` added by @MichaReiser on 2024-11-06 13:21_

---

_Comment by @dylwil3 on 2024-11-06 13:23_

I agree with Micha - you could also surround the annotation with quotation marks ([playground](https://play.ruff.rs/bfd71d97-6d9a-4385-b945-6c571d3d8d65)).

---

_Comment by @dylwil3 on 2024-11-06 13:26_

But it sounds like you're seeing an undesired lint with your production code (which presumably can't have a syntax error in it!) Would it be possible to find a version of the more complex code that produces the undesired lint?

---

_Label `needs-mre` added by @dylwil3 on 2024-11-06 13:27_

---

_Comment by @JoranDox on 2024-11-06 13:31_

@MichaReiser Huh, I was literally just going to comment that pandas does literally this here https://github.com/pandas-dev/pandas/blob/v2.2.3/pandas/core/frame.py#L1805-L1931

but it doesn't trigger the same error, and you're right, they import annotations from __future__

Is that the intended way to do this? Python allows this as-is, right?

@dylwil3 Indeed, I realised now that quoting the annotation is exactly what `from __future__ import annotations` does lol
I'm not entirely understanding why this makes a difference, 

As for the production code, it's really very similar in structure, just with more arguments & other irrelevant things.
It gets the underline in vscode from ruff, but just runs fine with actual python.

---

_Comment by @dylwil3 on 2024-11-06 13:33_

What version of Python are you using? This is what I see for Python 3.11:

```bash
➜  ~ python
Python 3.11.5 (main, Sep  8 2023, 16:31:47) [Clang 14.0.3 (clang-1403.0.22.14.1)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> class Myclass:
...     def returnself(self) -> Myclass:
...         return self
...
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in Myclass
NameError: name 'Myclass' is not defined
```

---

_Comment by @JoranDox on 2024-11-06 13:37_

Huh, I think something is being weird here, you're right, running this example is indeed failing.
I'm afraid I accidentally found a hole in our tests :fearful: 
And I was mistaken, it's not yet production code, it's a refactor in progress pulling a function like this
```python

class Myclass:
    def __init__(self, params):
        ...

def create_myclass(params) -> Myclass:
    return Myclass(params)
```

into the class as a factory method, my bad.

All right, TIL about how annotations (don't) work in python, sorry for the confusion and thanks!

---

_Closed by @JoranDox on 2024-11-06 13:37_

---

_Comment by @dylwil3 on 2024-11-06 13:47_

No worries, glad we could sort out the issue!

---
