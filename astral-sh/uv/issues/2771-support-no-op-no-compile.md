---
number: 2771
title: Support no-op --no-compile
type: issue
state: closed
author: henryiii
labels:
  - compatibility
assignees: []
created_at: 2024-04-02T08:30:37Z
updated_at: 2024-06-23T17:21:27Z
url: https://github.com/astral-sh/uv/issues/2771
synced_at: 2026-01-10T01:23:22Z
---

# Support no-op --no-compile

---

_Issue opened by @henryiii on 2024-04-02 08:30_

It would be nice to simply ignore `--no-compile`, rather than throw an error, to make it easier to swap pip and uv.

---

_Referenced in [astral-sh/uv#2752](../../astral-sh/uv/issues/2752.md) on 2024-04-02 08:31_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-02 12:27_

---

_Label `compatibility` added by @charliermarsh on 2024-04-02 12:27_

---

_Comment by @charliermarsh on 2024-04-02 12:27_

Thanks, I assumed we did this already. Will add.

---

_Comment by @sam-goodwin on 2024-04-03 19:29_

I think I ran into this with `uv pip compile`. Just wanted a way to create a `requirements.txt` from `pyproject.toml` but it built `pyspark` which took 1-2 minutes. Expected the operation to be instant.

---

_Comment by @zanieb on 2024-04-03 20:02_

@sam-goodwin this is totally separate — `--no-compile` means do not compile `.pyc` files. You're looking for `--only-binary`.

---

_Comment by @henryiii on 2024-04-03 20:08_

That's not what `--no-compile` means. It means skip making .pyc files from .py files, which uv already does, and is opt-in for pip.

---

_Comment by @sam-goodwin on 2024-04-03 20:09_

Thanks @zanieb and @henryiii , sorry for the confusion.

---

_Referenced in [astral-sh/uv#2816](../../astral-sh/uv/pulls/2816.md) on 2024-04-03 20:26_

---

_Closed by @charliermarsh on 2024-04-03 20:36_

---

_Comment by @staaam on 2024-06-23 17:03_

@charliermarsh  I'd like to reopen this issue, since --no-compile flag is still not support. Getting this error:
```
$ uv pip install --no-compile test
error: unexpected argument '--no-compile' found

  tip: a similar argument exists: '--no_compile'
```
which kind of misses the point (tested on uv==0.2.13)

---

_Comment by @charliermarsh on 2024-06-23 17:14_

It's `--no-compile-bytecode`, but we can add an alias.

---

_Referenced in [astral-sh/uv#4452](../../astral-sh/uv/issues/4452.md) on 2024-06-23 17:16_

---

_Comment by @charliermarsh on 2024-06-23 17:21_

Fixed in https://github.com/astral-sh/uv/pull/4453.

---
