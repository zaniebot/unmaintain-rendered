---
number: 3423
title: Add ability to check files one by one and print which are currently checked/parsed
type: issue
state: closed
author: qarmin
labels:
  - cli
assignees: []
created_at: 2023-03-09T19:39:09Z
updated_at: 2023-03-13T21:14:09Z
url: https://github.com/astral-sh/ruff/issues/3423
synced_at: 2026-01-07T13:12:14-06:00
---

# Add ability to check files one by one and print which are currently checked/parsed

---

_Issue opened by @qarmin on 2023-03-09 19:39_

When trying to found file which crashes, I use binary search so I:
1. run ruff to check for crash
2. remove half of files
3. run again to see if crash still happens
4. if yes, back to point 2
5. if no, restore half of files, remove other part, back to point 3

With such output I will be able to find crash with seconds instead minutes(of course this should work only on 1 thread):
```
[[ Parsing a.py ]]
[[ Parsing b.py ]]
[[ Parsing c.py ]]
[[ Parsing d.py ]]

[[ Checking a.py file ]]
a.py:21:9: D400 [*] First line should end with a period
[[ Checking a.py file ]]
b.py:21:9: D415 [*] First line should end with a period, question mark, or exclamation point
[[ Checking c.py file ]]
[[ Checking d.py file ]]
```


---

_Comment by @charliermarsh on 2023-03-09 19:47_

The single-threading part is achievable by setting the `RAYON_NUM_THREADS=1` environment variable, but not sure that we log the checked files. We could add that logging to `--verbose`, and then `RAYON_NUM_THREADS=1 ruff ... --verbose` would enable this, I think.

---

_Label `cli` added by @charliermarsh on 2023-03-10 05:30_

---

_Referenced in [astral-sh/ruff#3489](../../astral-sh/ruff/pulls/3489.md) on 2023-03-13 20:03_

---

_Referenced in [astral-sh/ruff#3491](../../astral-sh/ruff/pulls/3491.md) on 2023-03-13 20:32_

---

_Closed by @charliermarsh on 2023-03-13 21:13_

---

_Comment by @charliermarsh on 2023-03-13 21:14_

You can now run with `RAYON_NUM_THREADS=1 ruff ... --verbose` to get this experience (as of next release, at least).

---
