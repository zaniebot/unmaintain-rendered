```yaml
number: 6025
title: "Bug with uppercase `# noqa: SIM114` and RUF100"
type: issue
state: closed
author: sondrfos
labels: []
assignees: []
created_at: 2023-07-24T08:29:37Z
updated_at: 2023-07-25T12:43:16Z
url: https://github.com/astral-sh/ruff/issues/6025
synced_at: 2026-01-12T15:54:45Z
```

# Bug with uppercase `# noqa: SIM114` and RUF100

---

_@sondrfos_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hi,

Thanks for making Ruff, it's an exellent linter :) 

We just recently updated our project to Ruff 0.0.280 and seems to experience a weird bug.
Our code uses a rather large if-statement pulled straight form a research paper. To keep the code and the paper pretty equal, the code is written the exact same way as the paper. This means that we need to ignore some SIM114-warnings. 

When updating to 0.0.280 we started to receive a warning
 ```RUF100 [*] Unused `noqa` directive (unused: `SIM114`)``` 
However, when removing the `# noqa: SIM114` we start to receive the warning:
```SIM114 Combine `if` branches using logical `or` operator```

When testing we discovered that if we changed to the comment to `# noqa: sim114` we dont receive the RUF100 anymore.

I'll include some small example code which shows the weird behaviour:

```python
a = True
b = False

def foo():
    if a > b: # noqa: SIM114
        return 3
    elif a == b:
        return 3
    elif a <  b: # noqa: sim114
        return 4
    elif b is None:
        return 4
```
The first `noqa` gives a RUF100 warning, the second gives none. 


---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-24 11:58_

---

_Comment by @charliermarsh on 2023-07-24 11:58_

Will take a look today!

---

_Comment by @charliermarsh on 2023-07-24 14:40_

Okay, it's a few confusing things working together... The root of the problem is that if branches contain different comments, we avoid flagging `SIM114`. So when you have this:

```python
def foo():
    if a > b: # noqa: SIM114
        return 3
    elif a == b:
        return 3
```

We _don't_ flag `SIM114` _because_ of the `# noqa: SIM114` comment, and so the comment then becomes unused. If you remove the comment, then `SIM114` is raised again, because the comment is no longer present to _make_ the branches non-identical.


---

_Comment by @charliermarsh on 2023-07-24 14:41_

I'm going to change the logic to ignore end-of-line comments and only check in-body comments, which will remove this confusion. (Your code with `# noqa: SIM114` at the end of the test lines should work as expected.)

---

_Closed by @charliermarsh on 2023-07-24 15:00_

---

_Comment by @sondrfos on 2023-07-25 12:43_

That makes sense. thanks for such quick responses! : )

---
