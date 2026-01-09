---
number: 12644
title: "`uv pip sync` misses dependencies vs `uv pip install`"
type: issue
state: closed
author: RafaelWO
labels:
  - question
assignees: []
created_at: 2025-04-03T06:46:23Z
updated_at: 2025-04-04T12:44:40Z
url: https://github.com/astral-sh/uv/issues/12644
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv pip sync` misses dependencies vs `uv pip install`

---

_Issue opened by @RafaelWO on 2025-04-03 06:46_

### Summary

Let your `requirements.txt` be as follows:

```txt
PyGithub==2.6.1
```

After creating a fresh virtual environment via `uv venv`, running `uv pip sync` resolves and installs only one package:

![Image](https://github.com/user-attachments/assets/7d508ec4-f36b-437f-a599-53d987e5db82)

But it misses [all dependencies of `PyGithub`](https://github.com/PyGithub/PyGithub/blob/main/pyproject.toml#L30).

Interestingly, `uv pip install` works:

![Image](https://github.com/user-attachments/assets/e928191c-9c0a-488d-af9f-919f0127b9cb)

When I run `uv pip sync` again, the dependencies of `PyGithub` are removed again.

![Image](https://github.com/user-attachments/assets/dbd4afea-058b-4b1c-be07-e1cea3790bd4)

### Platform

Ubuntu 24.04.2 LTS x86_64

### Version

uv 0.6.12

### Python version

Python 3.13.1

---

_Label `bug` added by @RafaelWO on 2025-04-03 06:46_

---

_Comment by @charliermarsh on 2025-04-03 12:55_

This is intentional. `uv pip sync` installs _exactly_ the dependencies in the input file -- it does not look at transitive dependencies (and removes anything extraneous). Typically, you'd run `uv pip compile`, then `uv pip sync` on the generated file.

---

_Label `bug` removed by @charliermarsh on 2025-04-03 12:55_

---

_Label `question` added by @charliermarsh on 2025-04-03 12:55_

---

_Comment by @RafaelWO on 2025-04-04 05:24_

Ah, of course! 😅 

I usually use the compile and sync workflow, but in this case, I was testing something and did not think about the fact that I should run sync on an auto-generated requirements file.

Would it make sense to expand the "help" with this info when running `uv pip sync --help`? Currently, it says

> Sync an environment with a `requirements.txt` file

I could imagine the following:

> Sync an environment with a `requirements.txt` file, i.e. installs packages without transient dependencies



---

_Comment by @charliermarsh on 2025-04-04 12:44_

I think I'm going to err on the side of leaving this as-is for now but revisiting if it comes up again as a point of user confusion. Sorry for the trouble!

---

_Closed by @charliermarsh on 2025-04-04 12:44_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-04 12:44_

---

_Referenced in [astral-sh/uv#12680](../../astral-sh/uv/issues/12680.md) on 2025-04-04 19:20_

---

_Referenced in [astral-sh/uv#14507](../../astral-sh/uv/issues/14507.md) on 2025-07-08 14:12_

---
