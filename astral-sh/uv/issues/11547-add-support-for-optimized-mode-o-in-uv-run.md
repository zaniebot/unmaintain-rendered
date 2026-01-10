---
number: 11547
title: "Add support for optimized mode (`-O`) in `uv run`"
type: issue
state: closed
author: gusutabopb
labels:
  - enhancement
assignees: []
created_at: 2025-02-16T02:19:34Z
updated_at: 2025-10-08T14:43:30Z
url: https://github.com/astral-sh/uv/issues/11547
synced_at: 2026-01-10T01:25:07Z
---

# Add support for optimized mode (`-O`) in `uv run`

---

_Issue opened by @gusutabopb on 2025-02-16 02:19_

### Summary

The standard CPython interpreter has a handful of flags that can be set when executing a Python program, most of which are not directly supported by `uv run`.

Do the uv maintainers have any plans to support additional flags, specifically the following optimization-related flags?

```
-O     : remove assert and __debug__-dependent statements; add .opt-1 before
         .pyc extension; also PYTHONOPTIMIZE=x
-OO    : do -O changes and also discard docstrings; add .opt-2 before
         .pyc extension
```


To be fair, I don't think it's absolutely necessary for this to be supported since the workaround (see examples below) is trivial and AFAICT `__debug__` is rarely used by regular Python users (I think it's mostly something used by the CPython team).

The only reason I brought this up is because *some* CPython flags are supported by `uv run`, namely `-m` from #7754, so I wondered if there are any plans to support additional CPython flags directly in `uv run`.
```
-m mod : run library module as a script (terminates option list)
```

### Example

#### `test.py`

```python
print(f"{__debug__=}")
assert 1 > 2
```

#### Current behavior
If the proposed feature is implemented, this would have the same output as the "Current workaround" below.
```bash
> uv run -O test.py
error: unexpected argument '-O' found

Usage: uv run [OPTIONS] [COMMAND]

For more information, try '--help'.
```

#### Current workaround (1)
```bash
> uv run python -O test.py
__debug__=False
```

#### Current workaround (2)
```bash
> PYTHONOPTIMIZE=1 uv run test.py
__debug__=False
```

#### Current behavior without `-O` 
For reference only. This is expected.
```bash
> uv run test.py   # or uv run python test.py
__debug__=True
Traceback (most recent call last):
  File "/Users/gustavo/uv_test/test.py", line 2, in <module>
    assert 1 > 2
           ^^^^^
AssertionError
```

<details>
<summary>Version info</summary>

```
> uv run python -V
Python 3.13.0
```

```
> uv run -V
uv-run 0.6.0 (591f38c25 2025-02-14)
```
</details>

---

_Label `enhancement` added by @gusutabopb on 2025-02-16 02:19_

---

_Comment by @charliermarsh on 2025-02-16 02:20_

Worth considering, thanks for raising!

---

_Comment by @zanieb on 2025-02-16 23:49_

I think we probably shouldn't add a flag for this since there are viable alternatives. I don't think we can match all the `python` flags.

---

_Closed by @charliermarsh on 2025-02-18 02:18_

---

_Comment by @charliermarsh on 2025-02-18 02:19_

Makes sense. Sorry we couldn't accommodate this @gusutabopb.

---

_Comment by @butterlyn on 2025-02-24 02:00_

Note that `-O` doesn't just set `__debug__` to `False`. It also disables all `assert` statements, which cannot (afaik) be easily incorporated into the workaround.

---

_Comment by @galenseilisnh on 2025-07-08 20:36_

Ah, unfortunate.

---

_Comment by @dunwoj-hlevel on 2025-10-08 14:43_

Is the viable alternative to just run python -0 in the virtual environment directly? Or what is the recommended approach here?

---
