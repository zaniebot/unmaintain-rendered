---
number: 6391
title: Local variable is assigned but never used (F841) cannot be ignored with dummy-variable-rgx in except block
type: issue
state: closed
author: phillipuniverse
labels:
  - needs-info
assignees: []
created_at: 2023-08-07T14:48:20Z
updated_at: 2024-12-04T23:20:35Z
url: https://github.com/astral-sh/ruff/issues/6391
synced_at: 2026-01-07T13:12:15-06:00
---

# Local variable is assigned but never used (F841) cannot be ignored with dummy-variable-rgx in except block

---

_Issue opened by @phillipuniverse on 2023-08-07 14:48_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Example:

```python
try:
    1 + 1
except Exception as _err:
    print("Problem")
```

Ruff always triggers `F841` on the `except Exception as _err` line:

```
F841 [*] Local variable `_` is assigned to but never used
```

Ruff version 0.0.282


---

_Comment by @charliermarsh on 2023-08-07 18:56_

Interestingly, Flake8 doesn't respect the dummy-variable-rgx here either. What's the use-case? Did this occur in a real example, where you want to assign the exception but not use it?

---

_Label `waiting-on-author` added by @charliermarsh on 2023-08-07 18:56_

---

_Comment by @phillipuniverse on 2023-08-07 19:08_

> Flake8 doesn't respect the dummy-variable-rgx here either.

Hm, that is interesting.

Originally I had written this in a project:

```python
try:
    1 + 1
except:
    logger.exception("Cannot add numbers :(")
```

And then I got feedback that it would be nice to be able to capture the caught exception in a variable for easy access in pdb and other debuggers. So I thought it would be simple to rewrite as:

```python
try:
    1 + 1
except Exception as _err:
    logger.exception("Cannot add numbers :(")
```

and was surprised when `as _err` got rewritten by the autofixer. What I ended up shipping was this:

```python
try:
    1 + 1
except Exception as err:
    logger.exception("Cannot add numbers :(", exc_info=err)
```

Maybe there is a slight benefit in this final solution, as `logger.exception(msg)` is equivalent to `logger.error(msg, exc_info=True)` which under the covers gets the original exception with `sys.exc_info()`.

So it's an easy workaround and maybe worthwhile to keep the exact same behavior as flake8 in this scenario.

---

_Comment by @tylerlaprade on 2023-08-07 19:39_

>it would be nice to be able to capture the caught exception in a variable for easy access in pdb and other debuggers

This is the tail wagging the dog, IMHO. My team complained about `RET504` removing the variable they introduced for the sole purpose of debugging. I held firm, as I don't believe we should allow the code to become messier for the sake of debugging practices.

---

_Comment by @charliermarsh on 2023-08-07 19:46_

Yeah, hmm... IMO it's strange to use that `as _err` pattern, but it's also surprising not to respect the dummy-variable-rgx here.

---

_Comment by @phillipuniverse on 2023-08-07 20:02_

> removing the variable they introduced for the sole purpose of debugging

@tylerlaprade if it makes you feel any better for holding firm, there are other ways to expose that return value.

In Pycharm you can check the 'Show Return Values' option

<img width="703" alt="image" src="https://github.com/astral-sh/ruff/assets/684275/d01cfc0e-7ad9-4192-a096-5adce1a385e3">

In pdb, you can use `__return__`, [detailed tip from SO](https://stackoverflow.com/questions/5073911/what-is-return/18674516#18674516)

---

_Referenced in [astral-sh/ruff#6492](../../astral-sh/ruff/pulls/6492.md) on 2023-08-11 03:52_

---

_Closed by @charliermarsh on 2023-08-11 04:02_

---

_Comment by @ghost on 2024-05-15 18:45_

I have a similar use case by for walrus assignment:

```python
    while _has_not_timed_out := (last_try_epoch_sec - start_epoch_sec) < timeout_sec:
        if _reached_retry_interval := (time() - last_try_epoch_sec) >= retry_interval_sec:
```

---

_Comment by @xames3 on 2024-12-04 23:20_

is this resolved or has been any possible workaround this issue? couldn't get it working with walrus operator either. below is my code sample

```python
def _strides_as_shape(
    shape: t.Sequence[int],
    itemsize: int,
) -> tuple[int, ...]:
    """Computes the memory strides for an array based on its shape and
    element size.
    """
    stride = itemsize
    return tuple((stride := stride * dim) // dim for dim in reversed(shape))[  # F841 local variable 'stride' is assigned to but never used
        ::-1
    ]
```

---
