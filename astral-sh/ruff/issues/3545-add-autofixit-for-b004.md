```yaml
number: 3545
title: Add autofixit for B004 
type: issue
state: closed
author: Skylion007
labels:
  - good first issue
  - fixes
assignees: []
created_at: 2023-03-15T18:25:14Z
updated_at: 2023-07-16T01:32:23Z
url: https://github.com/astral-sh/ruff/issues/3545
synced_at: 2026-01-10T11:09:46Z
```

# Add autofixit for B004 

---

_Issue opened by @Skylion007 on 2023-03-15 18:25_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
This check seems autofixable with minimal chance of being an incorrect fixit. I would recommend enabling autofixes for this check by using `callable(x)` when possible instead of `hasattr(x, "__call__")`. Should be a good first issue for someone to implement.


---

_Comment by @Skylion007 on 2023-03-15 18:36_

Ah, I just found an edge case that makes this a bit more complex to autofix:
```
  
               func = getattr(self._f, '__call__', self._f.__init__)
                      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ B004
    
```
Is also flagged. I am not sure if this is a bug from flake8-bugprone or if it wants us to replace it with
```
               func = self._f if callable(f) else self._f.__init__
```


---

_Comment by @JonathanPlasse on 2023-03-15 19:25_

We could ignore the complex case.

---

_Label `good first issue` added by @charliermarsh on 2023-03-15 19:55_

---

_Label `autofix` added by @charliermarsh on 2023-03-15 19:55_

---

_Comment by @charliermarsh on 2023-03-15 19:55_

Agreed, we should be able to fix these.

---

_Comment by @glokta1 on 2023-03-17 11:43_

Hi, I'd like to take this up!

---

_Assigned to @glokta1 by @charliermarsh on 2023-03-17 19:26_

---

_Comment by @charliermarsh on 2023-03-17 19:26_

@glokta1 - Awesome! Please ask me here or on Discord if you have questions.

---

_Comment by @JonathanPlasse on 2023-03-17 19:43_

Unrelated.
How can I join the discord?

---

_Comment by @charliermarsh on 2023-03-17 19:47_

There's a link in the README! Clicking this should work: https://discord.com/invite/c9MhzV8aU5

---

_Closed by @charliermarsh on 2023-07-16 01:32_

---
