```yaml
number: 96
title: "Fix false positive for `IfTuple`"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: false-positive-if-tuple
created_at: 2022-09-04T01:34:12Z
updated_at: 2022-09-04T02:59:31Z
url: https://github.com/astral-sh/ruff/pull/96
synced_at: 2026-01-12T15:55:04Z
```

# Fix false positive for `IfTuple`

---

_@harupy_

This PR fixes a false positive for `IfTuple` when a tuple has no elements:

```bash
> cat resources/test/fixtures/F634.py
if (1, 2):
    pass

for _ in range(5):
    if True:
        pass
    elif (3, 4):
        pass
    elif ():
        pass


# BEFORE
> cargo run resources/test/fixtures/F634.py
    Finished dev [unoptimized + debuginfo] target(s) in 0.06s
     Running `target/debug/ruff resources/test/fixtures/F634.py`
Found 3 error(s).

resources/test/fixtures/F634.py:1:1: F634 If test is a tuple, which is always `True`
resources/test/fixtures/F634.py:7:5: F634 If test is a tuple, which is always `True`
# false positive - empty tuple is falsy
resources/test/fixtures/F634.py:9:5: F634 If test is a tuple, which is always `True`

# AFTER
> cargo run resources/test/fixtures/F634.py
   Compiling ruff v0.0.25 (/home/haru/Desktop/repositories/ruff)
    Finished dev [unoptimized + debuginfo] target(s) in 2.77s
     Running `target/debug/ruff resources/test/fixtures/F634.py`
Found 2 error(s).

resources/test/fixtures/F634.py:1:1: F634 If test is a tuple, which is always `True`
resources/test/fixtures/F634.py:7:5: F634 If test is a tuple, which is always `True`
```

`IfTuple` implementation in `pyflakes`:

https://github.com/PyCQA/pyflakes/blob/7d6479e46f8f3e9607c9ef7975cc892db023d413/pyflakes/checker.py#L1912-L1915

```python
    def IF(self, node):
        if isinstance(node.test, ast.Tuple) and node.test.elts != []:
            self.report(messages.IfTuple, node)
        self.handleChildren(node)
```

Tuple emptiness is checked as well (`node.test.elts != []`).


---

_@charliermarsh approved on 2022-09-04 02:39_

Great PR, thank you!

---

_Merged by @charliermarsh on 2022-09-04 02:56_

---

_Closed by @charliermarsh on 2022-09-04 02:56_

---

_Branch deleted on 2022-09-04 02:59_

---
