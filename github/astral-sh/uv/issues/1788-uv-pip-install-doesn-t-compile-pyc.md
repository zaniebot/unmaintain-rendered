---
number: 1788
title: "`uv pip install` doesn't compile pyc"
type: issue
state: closed
author: hauntsaninja
labels:
  - enhancement
  - compatibility
assignees: []
created_at: 2024-02-21T00:03:13Z
updated_at: 2024-03-05T03:35:25Z
url: https://github.com/astral-sh/uv/issues/1788
synced_at: 2026-01-07T13:12:16-06:00
---

# `uv pip install` doesn't compile pyc

---

_Issue opened by @hauntsaninja on 2024-02-21 00:03_

pip does this by default (you can opt out with `--no-compile`). Without this, you pay the cost on first import, which is undesirable in various situations. (Also a thing to make sure is apples to apples when benchmarking)

---

_Label `enhancement` added by @zanieb on 2024-02-21 00:22_

---

_Label `compatibility` added by @zanieb on 2024-02-21 00:22_

---

_Comment by @konstin on 2024-02-21 11:31_

We can add a `--compile` flag like poetry has (https://python-poetry.org/docs/cli/#install at the bottom).

The original motivation for this was that with bytecode compiling enabled, the majority of installation time in my benchmarks was spent on compiling.

> Without this, you pay the cost on first import, which is undesirable in various situations.

Could you elaborate?

---

_Comment by @AlexWaygood on 2024-02-21 12:04_

> > Without this, you pay the cost on first import, which is undesirable in various situations.
> 
> Could you elaborate?

Whenever you import a `.py` file in Python, if Python doesn't see an associated `.pyc` file for that `.py`, the first time it is imported by Python, it will compile the `.py` module into a `.pyc` file and save that onto the disk (in a `__pycache__` directory). That means that the first time you use that module, the import will be much slower than any subsequent times the module is used. This is especially undesirable for e.g. a CLI tool. It means the first time the CLI tool is used will be much slower than any subsequent usages of the CLI tool, since a significant amount of time in the first usage will be spent by Python compiling all the Python code into `.pyc` files. After the first usage, Python will just reuse the `.pyc` files from the `__pycache__` directory, so you "only pay the cost on the first import", as @hauntsaninja says.

If you're an author of a CLI tool, it's desirable for the installer to pre-compile the Python files into `.pyc` files as part of the installation process, so that performance of the CLI tool is consistent from the start

---

_Comment by @hauntsaninja on 2024-02-21 20:18_

Yup, like Alex says, this is undesirable in any case you're sensitive to import latency at runtime.

Another example is if you run your installs when building a container, you will now pay the cost every single time you start a job with that container, instead of just once upfront. This is especially unfortunate if you're running a relatively short lived process in that container.

I know it makes benchmarks look worse (although in that case you should be comparing to `pip --no-compile` as your baseline), but this is genuinely useful time that users will likely be paying either way! Feels like generally a good use of performance envelope. I do think uv would be able to compile faster than pip does, a) pip does not do any multiprocessing, b) you have the opportunity to do copy-on-write for pyc files (maybe modulo some python version stuff)

---

_Comment by @charliermarsh on 2024-02-21 20:20_

Makes sense!

---

_Comment by @thundergolfer on 2024-02-26 18:54_

Will +1 @hauntsaninja's comments about added container startup latency. At [modal](https://modal.com) we've found that when poetry's `--compile` flag is not used at build-time the resulting lack of `.pyc` files can add multiple seconds of startup latency for large packages: https://github.com/modal-labs/modal-client/pull/1112.

---

_Referenced in [astral-sh/uv#1928](../../astral-sh/uv/issues/1928.md) on 2024-02-28 12:14_

---

_Assigned to @konstin by @konstin on 2024-02-29 15:54_

---

_Referenced in [astral-sh/uv#2086](../../astral-sh/uv/pulls/2086.md) on 2024-02-29 16:33_

---

_Comment by @charliermarsh on 2024-02-29 17:25_

@konstin is on it.

---

_Closed by @charliermarsh on 2024-03-05 03:35_

---

_Referenced in [astral-sh/uv#2528](../../astral-sh/uv/issues/2528.md) on 2024-03-18 23:18_

---

_Referenced in [astral-sh/uv#2637](../../astral-sh/uv/issues/2637.md) on 2024-03-24 03:54_

---
