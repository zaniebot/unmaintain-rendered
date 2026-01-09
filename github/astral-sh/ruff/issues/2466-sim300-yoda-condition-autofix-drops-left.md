---
number: 2466
title: "SIM300: Yoda condition... autofix drops left parenthesis"
type: issue
state: closed
author: Jeremiah-England
labels:
  - bug
assignees: []
created_at: 2023-02-02T04:28:55Z
updated_at: 2023-02-02T05:07:45Z
url: https://github.com/astral-sh/ruff/issues/2466
synced_at: 2026-01-07T13:12:14-06:00
---

# SIM300: Yoda condition... autofix drops left parenthesis

---

_Issue opened by @Jeremiah-England on 2023-02-02 04:28_

```sh
ruff --select SIM300 --fix --diff test.py
```

The first parenthesis is dropped in the autofix here and the second parenthesis ends up in he wrong place. 

```diff
--- test.py
+++ test.py
@@ -1,3 +1,3 @@
 number = 1
-if 0 < (number - 100):
+if number - 100 > 0):
     print(number)
```

It should be like this:

```diff
--- test.py
+++ test.py
@@ -1,3 +1,3 @@
 number = 1
-if 0 < (number - 100):
+if (number - 100) > 0:
     print(number)
```

Ruff version: `0.0.239`.

Also, thanks for working on Ruff! It's super useful.

Edit: I see now you could strip out the parenthesis in that example completely. But that seems like a job for separate formatting rule. And my real-life example involved a walrus operator `:=` where the parenthesis were necessary.

---

_Comment by @charliermarsh on 2023-02-02 04:31_

Woah, very strange! Taking a look.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-02 04:31_

---

_Label `bug` added by @charliermarsh on 2023-02-02 04:31_

---

_Comment by @charliermarsh on 2023-02-02 04:31_

I have a few more minutes before bed so maybe I can fix it tonight :joy:

---

_Referenced in [astral-sh/ruff#2468](../../astral-sh/ruff/pulls/2468.md) on 2023-02-02 04:56_

---

_Closed by @charliermarsh on 2023-02-02 05:07_

---
