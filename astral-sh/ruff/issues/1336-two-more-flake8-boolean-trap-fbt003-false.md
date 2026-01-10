```yaml
number: 1336
title: Two more flake8-boolean-trap FBT003 false positives
type: issue
state: closed
author: rhkleijn
labels: []
assignees: []
created_at: 2022-12-22T15:49:40Z
updated_at: 2022-12-22T15:56:05Z
url: https://github.com/astral-sh/ruff/issues/1336
synced_at: 2026-01-10T12:05:26Z
```

# Two more flake8-boolean-trap FBT003 false positives

---

_Issue opened by @rhkleijn on 2022-12-22 15:49_

I highly appreciate  your amazing work on ruff!

I noticed your recent PR #1333 . In my code base I encountered two more flake8-boolean-trap FBT003 false positives for `getattr` and for the `index` method of `list`, `tuple` and other Sequences. E.g.

```python
getattr(someobj, attrname, False)  # return False if `attrname` is not found on `someobj`
mylist.index(True)  # find index of first occurrence of True
```

It would be great to get rid of some `# noqa: FBT003` comments for these cases.

---

_Comment by @charliermarsh on 2022-12-22 15:51_

Will add!

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-22 15:55_

---

_Closed by @charliermarsh on 2022-12-22 15:56_

---
