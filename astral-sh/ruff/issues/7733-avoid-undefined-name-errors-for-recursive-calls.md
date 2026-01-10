```yaml
number: 7733
title: "Avoid `undefined-name` errors for recursive calls after deletions"
type: issue
state: open
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-10-01T04:05:55Z
updated_at: 2024-07-19T14:33:08Z
url: https://github.com/astral-sh/ruff/issues/7733
synced_at: 2026-01-10T11:09:50Z
```

# Avoid `undefined-name` errors for recursive calls after deletions

---

_Issue opened by @charliermarsh on 2023-10-01 04:05_

Given:

```python
def func(x: int):
    if x > 0:
        func(x - 1)
    print(x)

func(1)
del func
```

Ruff reports an undefined name (F821) on the inner `func` call, because it's deleted in the outer scope. This is hard to solve in general, but given that the function must exist in order for it to be called, we should be able to do better here.

See: https://github.com/python-trio/trio/pull/2795#discussion_r1341908880.


---

_Label `bug` added by @charliermarsh on 2023-10-01 04:05_

---

_Renamed from "Avoid undefined-name errors for recursive calls after deletions" to "Avoid `undefined-name` errors for recursive calls after deletions" by @charliermarsh on 2023-10-01 04:06_

---

_Comment by @A5rocks on 2023-10-01 05:44_

I suppose that a quick sanity check for whatever solution (if there is one) is that this should still fail...
```py
>>> def f(x):
...   if x: f(False)
...
>>> z = f
>>> z(True)
>>> del f
>>> z(True)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in f
NameError: name 'f' is not defined
```

... but I think as long as you just assume the worst if reassignment happens, then this shouldn't have other issues.

---

_Comment by @joaoleveiga on 2024-07-19 14:33_

Any recommendations here? I am not sure if I should open a new issue or not but should this feature capture situations such as:

```python
# script.py
X = "default value"

def main():
    def f():
        print(X)

    if False:
        X = "new value"
    f()

main()
```

```
$ python script.py
NameError: free variable 'X' referenced before assignment in enclosing scope
```

---
