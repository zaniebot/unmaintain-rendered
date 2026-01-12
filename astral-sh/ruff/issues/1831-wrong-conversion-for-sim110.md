```yaml
number: 1831
title: Wrong conversion for SIM110
type: issue
state: closed
author: hoxbro
labels:
  - bug
assignees: []
created_at: 2023-01-12T20:53:05Z
updated_at: 2023-01-12T21:26:46Z
url: https://github.com/astral-sh/ruff/issues/1831
synced_at: 2026-01-12T15:54:41Z
```

# Wrong conversion for SIM110

---

_@hoxbro_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->
I hope you don't get tired of hearing it, but great work you and the other contributors are doing :+1: 

I have this small example code that simplifies the code a bit too much. 

``` python
def sim110_error(vals):
    for val in vals:
        if isinstance(val, dict):
            return True
        elif isinstance(val, list):
            return True
    return False
```

When running `ruff file.py --isolated --select=SIM` it return ```SIM110 Use `return any(isinstance(val, dict) for val in vals)` instead of `for` loop```, my version is 0.0.219.

No simplification is done if I comment out the `return False`. 


---

_Comment by @charliermarsh on 2023-01-12 20:55_

Thank you! I really appreciate it! And it always helps to hear :)

Yeah, this looks wrong. Let me take a look...

---

_Label `bug` added by @charliermarsh on 2023-01-12 20:55_

---

_Comment by @charliermarsh on 2023-01-12 20:57_

(Also: great issue, thanks for including a clear test case and using `--isolated`...)

---

_Comment by @charliermarsh on 2023-01-12 20:59_

Looks like `flake8-simplify` has the same problem:

```
❯ flake8 foo.py --isolated --select SIM
foo.py:2:5: SIM110 Use 'return any(isinstance(val, dict) for val in vals)'
```

(Which doesn't mean we shouldn't fix it.)

---

_Comment by @hoxbro on 2023-01-12 21:09_

> (Which doesn't mean we shouldn't fix it.)

Sounds good. 

Just noting that adding a nested if-statement will still simplify the problem. 

``` python
def sim110_error(vals):
    for val in vals:
        if isinstance(val, dict):
            return True
        elif isinstance(val, list):
            if len(val) > 1:
                return True
    return False
``` 

---

_Closed by @charliermarsh on 2023-01-12 21:17_

---

_Comment by @charliermarsh on 2023-01-12 21:17_

Will go out in a bit, as part of v0.0.220.

---

_Comment by @charliermarsh on 2023-01-12 21:26_

I'd like to also simplify this case, which we currently miss:

```py
for val in vals:
    if isinstance(val, dict):
        return True
else:
    return False
```

---
