---
number: 5035
title: "PyUpgrade Rules: Respect `target-version`"
type: issue
state: closed
author: smsegal
labels:
  - question
assignees: []
created_at: 2023-06-12T19:04:18Z
updated_at: 2023-06-12T19:49:20Z
url: https://github.com/astral-sh/ruff/issues/5035
synced_at: 2026-01-07T13:12:15-06:00
---

# PyUpgrade Rules: Respect `target-version`

---

_Issue opened by @smsegal on 2023-06-12 19:04_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

We need to set our `target-version` to `py37` in our codebase. When enabling the `UP` ruleset, `ruff` will use `pyupgrade` rules that are invalid for python 3.7. 

An example: 

```toml
# pyproject.toml snippet
[tool.ruff]
target-version = "py37"
select = ["UP"]
```

```python3
from typing import Tuple

# ruff reports "UP006: Use tuple instead of Tuple"
def foo(x: int, y: int) -> Tuple[int, int]: 
    return (x, y)
```

This is invalid for python versions below 3.9 (see the deprecation notice at https://docs.python.org/3.11/library/typing.html#typing.Tuple)

Details: 

Using `ruff 0.0.272` on MacOS, but reproduces on linux hosts as well. 

Command to reproduce using the above python snippet: 

```bash
ruff check --isolated --select "UP" --target-version "py37" foo.py
```

---

_Comment by @charliermarsh on 2023-06-12 19:08_

Hmm, I ran that exact command (`ruff check --isolated --select "UP" --target-version "py37" foo.py -n`), on that exact snippet, and didn't get any `UP006` error.

Do you perhaps have `from __future__ import annotations` in that file? Or is it a `.pyi` file?

---

_Label `question` added by @charliermarsh on 2023-06-12 19:08_

---

_Comment by @smsegal on 2023-06-12 19:10_

Ah, good catch! I didn't notice, but we do indeed have `__future__` annotations in that file. 
Curious, why do the `__future__` annotations cause this? 

---

_Comment by @smsegal on 2023-06-12 19:11_

And yeah, sure enough removing the `__future__` imports fixes this. 

---

_Comment by @charliermarsh on 2023-06-12 19:16_

`__future__` annotations effectively cause your type annotations to be treated as strings at runtime. So when Python runs your file, it doesn't try to parse / interpret the `tuple[int, int]` annotation at all -- it just stores it as a string. Static analysis tools can then grab that expression, parse it, and do whatever analysis they need. Because these static analysis tools don't need to be tied to your Python version, it's possible to use language features within annotations (like `tuple[int, int]`) that aren't yet supported by your minimum Python _runtime_ version.

So, this _is_ expected (and it's safe, as long as you're using `__future__` annotations). I'd suggest turning off the rule entirely if you still don't want that transform, even when `__future__` annotations are present :)


---

_Comment by @charliermarsh on 2023-06-12 19:20_

Closing as "working as intended", but happy to answer any follow-up questions!

---

_Closed by @charliermarsh on 2023-06-12 19:20_

---

_Comment by @smsegal on 2023-06-12 19:28_

Thanks for the info! That makes total sense. Are you aware of any downsides of importing `__future__.annotations`? Reading more about it, I see the PEP introducing it was delayed twice "to avoid breaking downstream code" but reading the announcements of the delays doesn't give any concrete details about what kind of breakages I should be worried about. 


(Also, really appreciate the fast turnaround time on issues in this repo. Excited to see what else comes from astral!)

---

_Comment by @charliermarsh on 2023-06-12 19:49_

Totally!

The main downside, AFAIK, is that it can cause problems for libraries that _rely_ on accessing type annotations at runtime, like Pydantic and FastAPI. I think those libraries do support `__future__` annotations now, but there are still some quirks. For example, this will give you a runtime error if you're on < Python 3.9 (the `int | None` syntax introduced in Python 3.9):

```py
from __future__ import annotations

from pydantic import BaseModel

class Model(BaseModel):
    a: int | None

print(Model(a=1))
```

`a: int | None` is fine from Python's perspective in general , since you have `from __future__ import annotations`; but Pydantic has to resolve the type annotation to something useful to make it enforceable at runtime, and when it tries, you get a `TypeError`.

So, in short, if you use `__future__` annotations with those libraries, some annotations that Python can _parse_ without issue may explode at runtime.

---
