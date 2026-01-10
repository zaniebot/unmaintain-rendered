```yaml
number: 4282
title: "PLC1901 false positive in `__bool__` methods"
type: issue
state: closed
author: trag1c
labels: []
assignees: []
created_at: 2023-05-08T13:18:47Z
updated_at: 2023-09-10T16:09:01Z
url: https://github.com/astral-sh/ruff/issues/4282
synced_at: 2026-01-10T11:09:47Z
```

# PLC1901 false positive in `__bool__` methods

---

_Issue opened by @trag1c on 2023-05-08 13:18_

```py
class X:
    val: str

    def __bool__(self) -> bool:
        return self.val != ""
```
```sh
$ ruff check test.py --select=PLC1901
test.py:5:28: PLC1901 `self.val != ""` can be simplified to `self.val` as an empty string is falsey
Found 1 error.
```
Trying to correct this to `self.val` will lead to an error:
```
TypeError: __bool__ should return bool, returned str
```


---

_Comment by @MichaReiser on 2023-05-08 14:15_

Yeah, The "safe" fix for this rule should probably be `bool(self.val)`. But even that is not entirely safe, because it now no longer reads `self.val`, which could have side effects. 



---

_Comment by @trag1c on 2023-05-08 16:32_

In one case I specifically need the comparison :)

---

_Comment by @charliermarsh on 2023-05-08 18:07_

I dislike this rule, too many false positives.

---

_Comment by @karolinepauls on 2023-06-21 18:45_

This rule is fundamentally incorrect for assertions or assignments, since `None != ""` - unless it can infer that the variable is a string.


```
a = 0

def test_sth() -> None:
    assert a == ""
```

Command: `ruff --isolated --select PLC1901 a.py`

Output:
```
a.py:4:17: PLC1901 `a == ""` can be simplified to `not a` as an empty string is falsey
Found 1 error.
```

```
$ ruff --version
ruff 0.0.274
```

---

_Comment by @charliermarsh on 2023-06-21 18:48_

Yes, Pylint's implementation has the same problem:

```python
❯ pylint --load-plugins=pylint.extensions.emptystring foo.py
************* Module foo
foo.py:1:0: C0114: Missing module docstring (missing-module-docstring)
foo.py:1:0: C0104: Disallowed name "foo" (disallowed-name)
foo.py:1:0: C0103: Constant name "a" doesn't conform to UPPER_CASE naming style (invalid-name)
foo.py:3:0: C0116: Missing function or method docstring (missing-function-docstring)
foo.py:4:11: C1901: "a == ''" can be simplified to "not a" as an empty string is falsey (compare-to-empty-string)
```

I'm going to move this rule to the nursery so that it's explicitly opt-in.


---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-21 18:52_

---

_Closed by @charliermarsh on 2023-06-21 19:47_

---

_Comment by @adifelice-godaddy on 2023-07-31 21:28_

Looks like the rule was renamed/revisited:
https://pylint.pycqa.org/en/latest/user_guide/messages/convention/use-implicit-booleaness-not-comparison-to-string.html

---

_Comment by @bersbersbers on 2023-09-10 16:09_

Here's another example where this rule creates false positives with `numpy`:

```python
import numpy as np

array = np.array(("X", "", "X"), dtype=str)
indices = array == ""
print(indices)

print(not array)
```

At least in this case, `not array` raises a `ValueError`, so incorrect fixes become obvious (at least at runtime).

---
