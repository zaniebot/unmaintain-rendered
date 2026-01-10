---
number: 8786
title: PERF102 does not trigger when the loop var is used in a previous loop
type: issue
state: open
author: ThiefMaster
labels:
  - bug
assignees: []
created_at: 2023-11-20T13:45:31Z
updated_at: 2024-02-13T14:40:10Z
url: https://github.com/astral-sh/ruff/issues/8786
synced_at: 2026-01-10T01:22:48Z
---

# PERF102 does not trigger when the loop var is used in a previous loop

---

_Issue opened by @ThiefMaster on 2023-11-20 13:45_

```py
dict_a = {}
dict_b = {}

for loopvar in {}:
    print(dict_a[loopvar])

for loopvar, val in dict_b.items():
    print(val)

for newloopvar, val in dict_b.items():
    print(val)
```

```
$ ruff --isolated --select B007,PERF102 --no-cache rufftest/ruff_sample.py
rufftest/ruff_sample.py:7:5: B007 Loop control variable `loopvar` not used within loop body
rufftest/ruff_sample.py:10:5: B007 Loop control variable `newloopvar` not used within loop body
rufftest/ruff_sample.py:10:24: PERF102 When using only the values of a dict use the `values()` method
Found 3 errors.
No fixes available (2 hidden fixes can be enabled with the `--unsafe-fixes` option).
```

---

_Renamed from "PERF102 does not trigger when the loop var is used previously" to "PERF102 does not trigger when the loop var is used in a previous loop" by @ThiefMaster on 2023-11-20 13:46_

---

_Comment by @zanieb on 2023-11-20 15:46_

Hm this seems to be a detail of the `SemanticModel::is_unused` implementation which will say a variable is used if it is shadowed. For example, this reproduces with the simpler:

```python
key = 1
print(key)

for key, val in {}.items():
    print(val)
```

---

_Comment by @zanieb on 2023-11-20 15:49_

And.. I think we can maybe be considered correct here although it is confusing because `key` is mutated:

```python
key = 1
print(key)

for key, val in {"x": 1, "y": 2}.items():
    print(val)

print(key)
```
```
1
1
2
y
```

I guess we could check if it's used _after_ the loop but that feels like a separate rule?

---

_Comment by @ThiefMaster on 2023-11-20 15:52_

Which, to be fair, is behavior nobody should ever rely upon ;)

TBH, maybe there should be a new rule that warns if you rely on the loop variable leaking outside the loop...

---

_Comment by @T-256 on 2023-11-20 15:56_

> I guess we could check if it's used _after_ the loop

With given example above, PERF102 didn't get line 7. I think that's the expected behavior.

---

_Label `bug` added by @charliermarsh on 2024-01-05 04:41_

---

_Comment by @mikeleppane on 2024-02-13 12:52_

I am working on this.

---

_Assigned to @mikeleppane by @dhruvmanila on 2024-02-13 13:09_

---

_Comment by @dhruvmanila on 2024-02-13 13:12_

> With given example above, PERF102 didn't get line 7. I think that's the expected behavior.

Can you explain why? `PERF102` isn't triggered for the second loop even though the `loopvar` variable remains unused within the body of that for loop.

---

_Comment by @mikeleppane on 2024-02-13 14:24_


> And.. I think we can maybe be considered correct here although it is confusing because `key` is mutated:
> 
> ```python
> key = 1
> print(key)
> 
> for key, val in {"x": 1, "y": 2}.items():
>     print(val)
> 
> print(key)
> ```
> 
> ```
> 1
> 1
> 2
> y
> ```
> 
> I guess we could check if it's used _after_ the loop but that feels like a separate rule?

Yep, perhaps the only way to solve this is to ignore possible bindings/references before the `for statement`. However, I'm not sure whether that requires a new rule? I'm fixing this to the current rule but if you disagree then I guess we can think about a new approach. 

---

_Referenced in [astral-sh/ruff#9978](../../astral-sh/ruff/pulls/9978.md) on 2024-02-13 20:53_

---

_Referenced in [astral-sh/ruff#14113](../../astral-sh/ruff/issues/14113.md) on 2024-11-05 17:24_

---
