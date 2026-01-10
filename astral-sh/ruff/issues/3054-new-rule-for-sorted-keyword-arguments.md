```yaml
number: 3054
title: new rule for sorted keyword arguments
type: issue
state: open
author: spaceone
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-02-20T10:38:30Z
updated_at: 2023-07-10T01:33:07Z
url: https://github.com/astral-sh/ruff/issues/3054
synced_at: 2026-01-10T11:09:46Z
```

# new rule for sorted keyword arguments

---

_Issue opened by @spaceone on 2023-02-20 10:38_

A new rule would be nice which can sort(autofix) keyword arguments in a consistent way - best in the order in which the function declares them:
```python
def foo(*, foo=1, bar=2):
    pass
foo(bar=3, foo=4)
```

→
```python
def foo(*, foo=1, bar=2):
    pass
foo(foo=4, bar=3)
```

where `foo(…)` in our case is `argparse.ArgumentParser.add_argument(…)`

---

_Label `rule` added by @charliermarsh on 2023-02-20 14:40_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:33_

---
