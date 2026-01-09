---
number: 4320
title: "Bug: `C419` autofix removing `list` within `any`/`all` causes undesirable short circuiting"
type: issue
state: closed
author: jamesbraza
labels:
  - fixes
assignees: []
created_at: 2023-05-09T18:22:53Z
updated_at: 2023-05-09T19:14:25Z
url: https://github.com/astral-sh/ruff/issues/4320
synced_at: 2026-01-07T13:12:14-06:00
---

# Bug: `C419` autofix removing `list` within `any`/`all` causes undesirable short circuiting

---

_Issue opened by @jamesbraza on 2023-05-09 18:22_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Please see the below code:

```python
class Worker:
    """Asynchronous worker."""

    def wait(self) -> bool:
        """Wait for worker thread to join."""
        # Code wraps threading.Event.wait()


def wait_all(*workers: Worker) -> bool:
    return all([w.wait() for w in workers])  # List stops short circuit
```

With `ruff==0.0.265`, and running `ruff --select=C419 --fix a.py`, it removes the `list` cast:

```python
def wait_all(*workers: Worker) -> bool:
    return all(w.wait() for w in workers)  # List stops short circuit
```

However, this actually added a subtle bug, because now the `all` statement will be short-circuited.  This short circuiting (in my scenario here) will cause all workers beyond the first `w.wait()` return of `False` to not be awaited.

I think `C4` autofixing should not be applied within `all`/`any`, as these can be short circuited, which can cause undesirable side effects.

---

_Label `autofix` added by @charliermarsh on 2023-05-09 18:51_

---

_Comment by @charliermarsh on 2023-05-09 18:53_

Ah yeah, this is intentional. In the future we'll probably qualify this as a suggested, and not an automated fix, since it changes the semantics of the program, but I wouldn't want to remove it entirely, since this is the entire rule and it's generally the correct fix.

---

_Closed by @charliermarsh on 2023-05-09 18:53_

---

_Comment by @zanieb on 2023-05-09 18:55_

We can make that change as part of https://github.com/charliermarsh/ruff/issues/4181, specifically https://github.com/charliermarsh/ruff/issues/4184 which should be ready to work on once #4303 is merged.

---

_Comment by @jamesbraza on 2023-05-09 19:14_

> but I wouldn't want to remove it entirely, since this is the entire rule and it's generally the correct fix.

Just to share my two cents here, I agree removing extra `list` casts is generally the correct fix, especially with argument unpacking into `*args`.

I would argue though, when short circuiting can happen, that `C4` actually changes the core behavior.  Imo autofixes should not be able to change fundamental behaviors.

---

_Referenced in [astral-sh/ruff#4424](../../astral-sh/ruff/pulls/4424.md) on 2023-05-14 01:33_

---
