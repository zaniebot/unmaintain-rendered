---
number: 1563
title: "`uv pip install` gets stuck combining `--resolution=lowest-direct` and `--upgrade` flags"
type: issue
state: closed
author: adamtheturtle
labels:
  - bug
  - resolver
assignees: []
created_at: 2024-02-17T02:48:11Z
updated_at: 2024-02-17T18:53:40Z
url: https://github.com/astral-sh/uv/issues/1563
synced_at: 2026-01-07T13:12:16-06:00
---

# `uv pip install` gets stuck combining `--resolution=lowest-direct` and `--upgrade` flags

---

_Issue opened by @adamtheturtle on 2024-02-17 02:48_

Tested with `uv 0.1.3`.

Choose a package, in this example, `mypy`.

Run:

```
uv pip install --resolution=lowest-direct --upgrade "mypy"
```

This stalls for at least 30 minutes.

Seen on macOS (locally) and on Ubuntu (GitHub Actions).

---

_Comment by @charliermarsh on 2024-02-17 02:52_

I think you can reproduce with just: `cargo run pip install --resolution=lowest-direct "wheel==0.1"`

---

_Referenced in [adamtheturtle/sphinx-substitution-extensions#679](../../adamtheturtle/sphinx-substitution-extensions/pulls/679.md) on 2024-02-17 03:21_

---

_Label `bug` added by @zanieb on 2024-02-17 04:08_

---

_Label `resolver` added by @zanieb on 2024-02-17 04:08_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-17 18:23_

---

_Referenced in [astral-sh/uv#1607](../../astral-sh/uv/pulls/1607.md) on 2024-02-17 18:31_

---

_Closed by @charliermarsh on 2024-02-17 18:53_

---
