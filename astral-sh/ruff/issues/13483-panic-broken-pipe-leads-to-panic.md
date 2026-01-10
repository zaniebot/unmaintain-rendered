```yaml
number: 13483
title: "[Panic] broken pipe leads to panic"
type: issue
state: closed
author: mitsuhiko
labels:
  - bug
assignees: []
created_at: 2024-09-23T13:21:53Z
updated_at: 2024-09-23T13:48:45Z
url: https://github.com/astral-sh/ruff/issues/13483
synced_at: 2026-01-10T11:09:55Z
```

# [Panic] broken pipe leads to panic

---

_Issue opened by @mitsuhiko on 2024-09-23 13:21_

```
ruff analyze graph --preview| bat -l json --color=always
```

Hit quit, be presented with a broken pipe:

```
error: Ruff crashed. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

...quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread 'main' panicked at library/std/src/io/stdio.rs:1118:9:
failed printing to stdout: Broken pipe (os error 32)
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Classic rust command line tool behavior :)

---

_Comment by @charliermarsh on 2024-09-23 13:22_

Lol. A duplicate of #13442.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-23 13:24_

---

_Label `bug` added by @charliermarsh on 2024-09-23 13:24_

---

_Comment by @charliermarsh on 2024-09-23 13:25_

I'll fix it now.

---

_Closed by @charliermarsh on 2024-09-23 13:48_

---
