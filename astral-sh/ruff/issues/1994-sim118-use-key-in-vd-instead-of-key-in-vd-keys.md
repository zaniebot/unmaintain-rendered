```yaml
number: 1994
title: "SIM118 Use `key in vd` instead of `key in vd.keys()` assumes `__iter__` is over `keys()`"
type: issue
state: closed
author: taldcroft
labels: []
assignees: []
created_at: 2023-01-19T11:13:29Z
updated_at: 2023-06-08T17:02:44Z
url: https://github.com/astral-sh/ruff/issues/1994
synced_at: 2026-01-10T11:09:44Z
```

# SIM118 Use `key in vd` instead of `key in vd.keys()` assumes `__iter__` is over `keys()`

---

_Issue opened by @taldcroft on 2023-01-19 11:13_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->
Example code:
```
class ValuesDict:
   """Dict-like container that iterates over the values."""
    def __init__(self, value):
        self.value = value

    def __iter__(self):
        yield from self.values()

    def keys(self):
        return self.value.keys()

    def values(self):
        return self.value.values()


d = {"a": 1, "b": 2, "c": 3}
vd = ValuesDict(d)

for key in vd.keys():
    print(key)
```
This is the original expected output:
```
$ python row.py
a
b
c
```
Applying fixes for SIM118:
```
$ ruff --select=SIM118 --fix row.py
Found 1 error(s) (1 fixed, 0 remaining).

$ git diff
diff --git a/row.py b/row.py
index 1e7536d..7751a3b 100644
--- a/row.py
+++ b/row.py
@@ -18,5 +18,5 @@ class ValuesDict:
 d = {"a": 1, "b": 2, "c": 3}
 vd = ValuesDict(d)
 
-for key in vd.keys():
+for key in vd:
     print(key)
```

This changes the code behavior:
```
$ python row.py                                 
1
2
3
```


---

_Comment by @not-my-profile on 2023-01-19 15:10_

Yes I think we currently have many lints that assume a conventional implementation of some `__dunder__` methods. I agree that we should categorize the applicability of such rules, I just opened #1997 for that :)

---

_Comment by @charliermarsh on 2023-01-19 15:15_

Yeah this is probably impossible to avoid completely right now. But we should categorize fixes as described in the issue and consider requiring opt-in for those that could be "dangerous".

---

_Comment by @taldcroft on 2023-01-19 17:59_

Thanks, sounds like a plan. This one certainly caught me by surprise.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-08 16:04_

---

_Closed by @charliermarsh on 2023-06-08 17:02_

---
