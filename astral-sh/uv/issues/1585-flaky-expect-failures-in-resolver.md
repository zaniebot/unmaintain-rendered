---
number: 1585
title: Flaky expect failures in resolver
type: issue
state: closed
author: hauntsaninja
labels:
  - bug
assignees: []
created_at: 2024-02-17T12:14:36Z
updated_at: 2024-02-21T20:46:20Z
url: https://github.com/astral-sh/uv/issues/1585
synced_at: 2026-01-10T01:23:07Z
---

# Flaky expect failures in resolver

---

_Issue opened by @hauntsaninja on 2024-02-17 12:14_

Getting the following segfault:
```
thread 'main' panicked at crates/uv-resolver/src/resolution.rs:135:30:
Every package should have metadata
```

Repro:
```
In [5]: Counter(subprocess.run("rm -rf .venv; uv venv --seed; uv pip install --no-cache --link-mode=copy --no-deps gpustat==1.1.1", shell=True).returncode for _ in range(100))
Out[5]: Counter({101: 73, 0: 27})
```

https://github.com/astral-sh/uv/pull/1583 should improve the error message (I had to do some bisection to narrow it down to a single package)

With #1583 it appears there's some connection to setuptools-scm

Similar message reported in this comment: https://github.com/astral-sh/uv/issues/1484#issuecomment-1948863469

---

_Referenced in [astral-sh/uv#1583](../../astral-sh/uv/pulls/1583.md) on 2024-02-17 12:15_

---

_Comment by @charliermarsh on 2024-02-17 12:23_

Awesome, thanks! Will fix.

---

_Label `bug` added by @charliermarsh on 2024-02-17 12:23_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-17 12:44_

---

_Comment by @charliermarsh on 2024-02-21 14:41_

I can reproduce this on some old commits (I randomly tried c1eb6130e1cb46a7ce5c34bccdfcb9245e29e8a8), but not on `main`, so I think it's resolved, at least on `main`? Will re-open if not.

---

_Closed by @charliermarsh on 2024-02-21 14:41_

---

_Comment by @hauntsaninja on 2024-02-21 20:44_

Thank you! I bisected the fix commit to #1607 , hopefully there's something in there that explains the formerly non-deterministic behaviour

---

_Referenced in [astral-sh/uv#2300](../../astral-sh/uv/issues/2300.md) on 2024-03-08 10:22_

---
