```yaml
number: 2008
title: PT007 raises a false positive for single-item strings
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2023-01-20T02:04:44Z
updated_at: 2023-01-20T03:13:19Z
url: https://github.com/astral-sh/ruff/issues/2008
synced_at: 2026-01-10T11:09:44Z
```

# PT007 raises a false positive for single-item strings

---

_Issue opened by @charliermarsh on 2023-01-20 02:04_

Not sure if this is inherited from the flake8 plugin, but this is a false positive for PT007:

```python
@pytest.mark.parametrize("archs", [["x86_64"], ["arm64", "universal2"]])
```

If the item on the left is a string with no commas, then the thing on the right doesn't have to be a list of tuples, it can be a list of anything, including lists.

_Originally posted by @henryiii in https://github.com/charliermarsh/ruff/issues/1506#issuecomment-1397813925_
            

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-20 02:50_

---

_Closed by @charliermarsh on 2023-01-20 03:13_

---
