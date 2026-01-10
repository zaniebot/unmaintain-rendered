```yaml
number: 9349
title: F821 false positive when deleting inner function after call inside of method
type: issue
state: open
author: Skylion007
labels:
  - bug
assignees: []
created_at: 2024-01-01T17:21:00Z
updated_at: 2024-01-02T18:07:11Z
url: https://github.com/astral-sh/ruff/issues/9349
synced_at: 2026-01-10T11:09:51Z
```

# F821 false positive when deleting inner function after call inside of method

---

_Issue opened by @Skylion007 on 2024-01-01 17:21_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
I recently enabled flake rule F821 on [PyTorch](https://github.com/pytorch/pytorch/pull/116579), but ran into a false positive: https://github.com/pytorch/pytorch/blob/bd10fea79a780b9ba7430c3434849fc7991f6db3/torch/nn/modules/module.py#L2127 . 

It was a bit of a difficult edge case, but I recreated a minimal repro here:
```python
class Dog:
    def f(self, a):
        c = a
        def load(b):
           b+=1

           if b < c+10:
               return load(b)
           return b


        d = load(a)
        del load
        return d
z = Dog().f(2000)
print(z)
assert z == 2010
```
This code is rather unorthodox, but valid. The load function is in scope before it's deleted and therefore the call to the inner recursive load function is defined during the first load call. This code fails the F821 rule even though the code is perfectly valid Python, so the symbols are not being parsed correctly.

Running `ruff --select F821 test_f821.py` on 0.1.9

outputs
```
test_f821.py:9:23: F821 Undefined name `load`
``` 

This in itself is not that severe of a bug, but I am worried that other rules may be affected if it cannot resolve recursive functions like this.

---

_Renamed from "F821 false positive when deleting inner function after call." to "F821 false positive when deleting inner function after call inside of method" by @Skylion007 on 2024-01-01 17:24_

---

_Label `bug` added by @charliermarsh on 2024-01-02 18:07_

---
