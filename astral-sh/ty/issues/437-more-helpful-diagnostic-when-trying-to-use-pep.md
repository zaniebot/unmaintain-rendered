```yaml
number: 437
title: More helpful diagnostic when trying to use PEP 604 union types before 3.10
type: issue
state: closed
author: metaist
labels:
  - diagnostics
assignees: []
created_at: 2025-05-18T03:41:09Z
updated_at: 2025-05-19T17:47:32Z
url: https://github.com/astral-sh/ty/issues/437
synced_at: 2026-01-12T15:54:23Z
```

# More helpful diagnostic when trying to use PEP 604 union types before 3.10

---

_@metaist_

### Summary

I believe this is distinct from #122 because even the most basic UnionType seems to fail.

```python
# test.py
from typing import Union

A = Union[int, str]
B = int | str
```

```bash
$ uvx ty check test.py

error[unsupported-operator]: Operator `|` is unsupported between objects of type `<class 'int'>` and `<class 'str'>`
 --> test.py:4:5
  |
3 | A = Union[int, str]
4 | B = int | str
  |     ^^^^^^^^^
  |
info: rule `unsupported-operator` is enabled by default
```

### Version

ty 0.0.1-alpha.5 and ty 0.0.1-alpha.5 (4ad13f211 2025-05-17)

---

_Comment by @sharkdp on 2025-05-18 06:10_

Thank you for reporting this.

If you set the Python version to 3.10 or higher, this should not lead to an error.

---

_Comment by @metaist on 2025-05-18 11:12_

Oh, right 🤦

`uvx --python 3.10 ty check test.py` didn't work, I now see the `--python-version` flag and the accompanying note
> ty will not infer the Python version from the Python environment at this time.

Maybe a `hint` or some other helpful message like mypy's (see below):
> `X | Y` syntax for unions requires running with `--python-version 3.10` or higher

I also noticed that `ty` *doesn't* complain about `UnionType` in function definitions even without `from __future__ import annotations`.

```python
def f(x: int|str) -> None: ...
```

```bash
$ uvx --python 3.9 mypy --strict test.py
test.py:1: error: X | Y syntax for unions requires Python 3.10  [syntax]
Found 1 error in 1 file (checked 1 source file)

$ uvx ty check --python-version 3.9 test.py
All checks passed!
```

```python
from __future__ import annotations
def f(x: int|str) -> None: ...
```

```bash
$ uvx --python 3.9 mypy --strict test.py
Success: no issues found in 1 source file

$ uvx ty check --python-version 3.9 test.py
All checks passed!
```

Not  sure if this should be a separate issue, so I'll close this one.

---

_Closed by @metaist on 2025-05-18 11:12_

---

_Comment by @sharkdp on 2025-05-19 08:06_

> Maybe a `hint` or some other helpful message like mypy's (see below):
> 
> > `X | Y` syntax for unions requires running with `--python-version 3.10` or higher

I can very much relate, since I ran into the exact same thing a few hours before you posted this issue, and it also took me a bit to figure out what was going on. Reopening this so we can keep track of this.

---

_Reopened by @sharkdp on 2025-05-19 08:06_

---

_Comment by @sharkdp on 2025-05-19 08:09_

> `uvx --python 3.10 ty check test.py` didn't work, I now see the `--python-version` flag and the accompanying note

So close! 😄

But seriously, I wonder if this should "just work", or if we should at least attempt to detect this discrepancy somehow and emit a warning? Haven't thought through it though. There might be legitimate use cases where one would want to run `uvx --python 3.10 ty check` even though the target Python version is 3.9.

---

_Comment by @sharkdp on 2025-05-19 08:20_

> I also noticed that `ty` _doesn't_ complain about `UnionType` in function definitions even without `from __future__ import annotations`.
> 
> def f(x: int|str) -> None: ...

This is a false negative, yes. Since it leads to an error at runtime, I think we should try to catch this. It would be great if you could open a new issue for this — thank you!

---

_Label `diagnostics` added by @sharkdp on 2025-05-19 08:21_

---

_Comment by @metaist on 2025-05-19 14:26_

> But seriously, I wonder if this should "just work", or if we should at least attempt to detect this discrepancy somehow and emit a warning? Haven't thought through it though. There might be legitimate use cases where one would want to run `uvx --python 3.10 ty check` even though the target Python version is 3.9.

`uv` has made it so easy for me to switch between python versions that I often forget which version I have active in the venv at any given time so `uvx --python <version>` has been my way of "forcing" a specific version when I really care.

So for `ty` my mental model is "use the active python version, unless I override with --python-version".

---

_Renamed from "Support PEP 604 UnionTypes" to "More helpful diagnostic when trying to use PEP 604 union types before 3.10" by @sharkdp on 2025-05-19 14:56_

---

_Comment by @carljm on 2025-05-19 15:48_

I think our fallback default for Python version is 3.13? So if you were getting 3.9 that implies you have a `pyproject.toml` which specifies that as your lower bound Python version? Is that correct?

I think it's plausible that we might add a fallback to whatever Python is on your shell path (which I think is the feature being discussed here), but it seems unlikely to me that we would give that priority over what's in the `pyproject.toml`.

---

_Comment by @metaist on 2025-05-19 16:13_

@carljm Yes, in my original issue I submitted I was working in a context with a `pyproject.toml` that has a lower bound of Python 3.9.

While both `mypy` and `pyright` look at the Python on the shell path for their default version, I do appreciate the idea of having `pyproject.toml` take priority where the implication is something like "this code should satisfies type checks for all versions of python you claim to support in your `pyproject.toml`".

This will impact my CI flow (which changes up the python version and re-runs all the linters/type checkers), but I'd learn to mentally rewrite: `uvx --python 3.9 ty check` to `uvx ty check --python-version 3.9` (and both `mypy` and `pyright` support equivalent `--python-version` flags).

---

_Comment by @MichaReiser on 2025-05-19 16:20_

> This will impact my CI flow (which changes up the python version and re-runs all the linters/type checkers), but I'd learn to mentally rewrite: uvx --python 3.9 ty check to uvx ty check --python-version 3.9 (and both mypy and pyright support equivalent --python-version flags).

What I have in mind for this use case is to distinguish between the lowest supported python version (which is what you specify with `requires-python`) and the version you currently check against. 

The lowest supported Python version would be used for syntax errors. The *current* Python version would be used for resolving types (would it type check when using Python XX). The current python verison could be inferred from the current used Python version.

---

_Comment by @AlexWaygood on 2025-05-19 16:54_

> What I have in mind for this use case is to distinguish between the lowest supported python version (which is what you specify with `requires-python`) and the version you currently check against.
> 
> The lowest supported Python version would be used for syntax errors. The _current_ Python version would be used for resolving types (would it type check when using Python XX). The current python verison could be inferred from the current used Python version.

I don't agree that this is a useful distinction for type checking. If my project supports Python 3.8, I want to know that it's going to work on Python 3.8 even if I'm using Python 3.13 for local development. Whether it's a syntax error or a type error doesn't make much difference here: if type checking fails on Python 3.8, that means that ty can't confirm that my code will run without error on Python 3.8, which is something I'd want to know about.

---

_Closed by @sharkdp on 2025-05-19 17:47_

---
