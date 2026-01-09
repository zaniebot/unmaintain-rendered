---
number: 4519
title: "[Autofix error] RUF001 autofix fails for `a = \"丶\"`"
type: issue
state: closed
author: konstin
labels:
  - fixes
assignees: []
created_at: 2023-05-19T08:02:44Z
updated_at: 2023-09-05T18:21:16Z
url: https://github.com/astral-sh/ruff/issues/4519
synced_at: 2026-01-07T13:12:14-06:00
---

# [Autofix error] RUF001 autofix fails for `a = "丶"`

---

_Issue opened by @konstin on 2023-05-19 08:02_

The RUF001 autofix replace

```
a = "丶"
```

with

```
a = "\"
```

which is a syntax error. For some reason ruff doesn't realize this is a syntax error, so you need e.g. `--select RUF001,UP025` to get a syntax error displayed.

Command:

```shell
target/debug/ruff --no-cache --select RUF001 --fix a.py
```

ruff version fd16d658e9407ce76bc38cd8f5112dd272fc5be0

This was originally discovered in https://github.com/bolucat/Firefox/blob/43f773cc70d6bc6322c2f085fa360957d7916bdb/third_party/python/idna/idna/uts46data.py


---

_Label `autofix` added by @konstin on 2023-05-19 08:02_

---

_Referenced in [astral-sh/ruff#4523](../../astral-sh/ruff/issues/4523.md) on 2023-05-19 09:14_

---

_Comment by @charliermarsh on 2023-05-19 12:31_

I think it's because this doesn't go through the AST checker, so we don't attempt to re-parse.

---

_Comment by @charliermarsh on 2023-05-19 12:52_

At the very least we should be checking if the replacement character is syntactically relevant within strings (a quote, a backslash... maybe some others).

---

_Referenced in [astral-sh/ruff#4533](../../astral-sh/ruff/pulls/4533.md) on 2023-05-19 17:59_

---

_Referenced in [astral-sh/ruff#6913](../../astral-sh/ruff/issues/6913.md) on 2023-08-27 13:44_

---

_Referenced in [astral-sh/ruff#3626](../../astral-sh/ruff/issues/3626.md) on 2023-08-28 20:02_

---

_Comment by @qarmin on 2023-09-05 17:33_

So is this still relevant after https://github.com/astral-sh/ruff/pull/7168 ?

---

_Comment by @charliermarsh on 2023-09-05 18:21_

No, thanks for noticing!

---

_Closed by @charliermarsh on 2023-09-05 18:21_

---
