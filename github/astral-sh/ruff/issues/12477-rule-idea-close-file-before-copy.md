---
number: 12477
title: "Rule idea: close file before copy"
type: issue
state: open
author: pjonsson
labels:
  - rule
assignees: []
created_at: 2024-07-23T14:58:29Z
updated_at: 2024-07-24T21:22:57Z
url: https://github.com/astral-sh/ruff/issues/12477
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule idea: close file before copy

---

_Issue opened by @pjonsson on 2024-07-23 14:58_

I recently came across the following (slightly simplified) pattern in two different code bases:
```python
import shutil

from requests import Session


def fun(session: Session, url: str) -> None:
  with session.get(url, stream=True) as response:
    file = "hello.txt"
    dst = "/tmp/otherfile.txt"
    with open(file, "wb") as f:
      for b in response.iter_content():
        f.write(b)
      shutil.copy(file, dst)
```
The issue is the `shutil.copy` being indented one level too much, so the copy is done before closing the file. Most of the time the code will work as intended, but sometimes the last kilobytes will be missing from the destination file (for the real code, ~100/250000 files were corrupted).

I'm guessing this kind of bug is typically for code in a single function, so perhaps matching on something like this would catch most cases in the wild:
```python
with open(file, "wb") as f:
  ...
  shutil.copy(file, dst)
```

```python
with tempfile.NamedTemporaryFile() as f:
  ...
  shutil.copy(f.name, dst)
```

---

_Label `rule` added by @charliermarsh on 2024-07-24 21:22_

---
