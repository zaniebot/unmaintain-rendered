---
number: 3943
title: "UP031 doesn't detect % on new line"
type: issue
state: closed
author: aslomoi
labels:
  - bug
assignees: []
created_at: 2023-04-12T03:40:57Z
updated_at: 2023-04-20T00:01:11Z
url: https://github.com/astral-sh/ruff/issues/3943
synced_at: 2026-01-07T13:12:14-06:00
---

# UP031 doesn't detect % on new line

---

_Issue opened by @aslomoi on 2023-04-12 03:40_

👋 

ruff 0.0.261

pyproject.toml
```
[tool.ruff]

select = [
    "UP"
]
```

Example:
```
var1 = "var 1"
var2 = "var 2"
print("this will be caught by UP031 with the variables: %s, %s" % (var1, var2))
print(
    "this has percent on a new line and will not be caught by UP031 with the variables: %s, %s"
    % (var1, var2)
)
```

Only the first print statement above results in UP031. 
The second one was formatted to split over two lines but doesn't raise UP031.


---

_Comment by @charliermarsh on 2023-04-12 03:43_

I _think_ we don't flag this because we're not yet capable of fixing it. But, we should probably flag it, even if we can't fix it.

---

_Label `bug` added by @charliermarsh on 2023-04-12 03:43_

---

_Referenced in [astral-sh/ruff#3953](../../astral-sh/ruff/pulls/3953.md) on 2023-04-13 03:38_

---

_Closed by @charliermarsh on 2023-04-13 03:59_

---

_Comment by @aslomoi on 2023-04-20 00:01_

Thanks for addressing this!

---
