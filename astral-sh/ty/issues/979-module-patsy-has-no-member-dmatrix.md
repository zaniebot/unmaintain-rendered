```yaml
number: 979
title: "Module `patsy` has no member `dmatrix`"
type: issue
state: closed
author: MamadouSDiallo
labels: []
assignees: []
created_at: 2025-08-13T22:42:26Z
updated_at: 2025-08-15T17:23:23Z
url: https://github.com/astral-sh/ty/issues/979
synced_at: 2026-01-10T02:06:24Z
```

# Module `patsy` has no member `dmatrix`

---

_Issue opened by @MamadouSDiallo on 2025-08-13 22:42_

### Summary

`ty` does not seem to know `dmatrix` is part of `patsy`.

```py
from patsy import dmatrix
```

### Version

_No response_

---

_Comment by @carljm on 2025-08-13 22:51_

This is because patsy's `__init__.py` uses a [highly dynamic method](https://github.com/pydata/patsy/blob/master/patsy/__init__.py#L79) to expose submodules, which isn't standardized for static analysis to understand. A quick check suggests no other Python type checker understands this either. It would require adding specialized support for patsy into type checkers.

A more feasible route would likely be for the patsy project (or any interested patsy user) to provide type stubs for patsy, or for patsy to use a less dynamic way of re-exporting submodule contents, if it wants to better support use with type-checking.

---

_Closed by @carljm on 2025-08-13 22:51_

---

_Comment by @MamadouSDiallo on 2025-08-15 17:11_

Mypy runs it just fine but thanks for the info. 

---

_Comment by @jelle-openai on 2025-08-15 17:15_

I suspect the reason mypy understands this is that it automatically treats submodules as available attributes on their parent modules, even if they're not explicitly exported. For similar reasons, on this code sample:

```python
import collections

print(collections.abc.Sequence)
```

ty [reports](https://play.ty.dev/cd6cc938-1134-491d-8ca7-248f4c4b040e), "Type `<module 'collections'>` has no attribute `abc`", but mypy is fine with it.

---

_Comment by @carljm on 2025-08-15 17:23_

> Mypy runs it just fine but thanks for the info.

I'm not able to reproduce that result. I put `from patsy import dmatrix` into `main.py` and then ran `uv init .; uv add patsy` to create a venv at `.venv` with patsy installed in it. Then I get these results from mypy:

```
➜ uvx mypy main.py --python-executable .venv/bin/python
main.py:1: error: Skipping analyzing "patsy": module is installed, but missing library stubs or py.typed marker  [import-untyped]
main.py:1: note: See https://mypy.readthedocs.io/en/stable/running_mypy.html#missing-imports
Found 1 error in 1 file (checked 1 source file)

➜ uvx mypy main.py --python-executable .venv/bin/python --follow-untyped-imports
main.py:1: error: Module "patsy" has no attribute "dmatrix"  [attr-defined]
Found 1 error in 1 file (checked 1 source file)
```

So initially mypy complains that patsy is untyped, and if I use `--follow-untyped-imports` it instead gives the same error ty does; it can't find `dmatrix` submodule either.

Mypy has lots of options, so it's quite possible that you have some combination of options that is silencing this error for you. But even if that's the case, I'm pretty sure that if you `reveal_type(dmatrix)` after `from patsy import dmatrix`, you'll find that mypy is just treating it as `Any`.


> I suspect the reason mypy understands this is that it automatically treats submodules as available attributes on their parent modules

I don't think that is the case here, because `dmatrix` is not actually a direct submodule of `patsy` at all; it's a more deeply-nested submodule that `patsy/__init__.py` explicitly attaches to the root `patsy` package using some dynamic code.

---
