```yaml
number: 4466
title: "False positive `PD008` when using `zipfile.Path.at`"
type: issue
state: closed
author: m-roberts
labels:
  - bug
assignees: []
created_at: 2023-05-17T08:08:10Z
updated_at: 2023-05-17T16:20:59Z
url: https://github.com/astral-sh/ruff/issues/4466
synced_at: 2026-01-10T11:09:47Z
```

# False positive `PD008` when using `zipfile.Path.at`

---

_Issue opened by @m-roberts on 2023-05-17 08:08_

`ruff==0.0.267` has a false positive `PD008`:

```python
import io
import zipfile


class MockBinaryFile(io.BytesIO):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)

    def close(self):
        pass  # Don't allow closing the file, it would clear the buffer


zip_buffer = MockBinaryFile()

with zipfile.ZipFile(zip_buffer, "w") as zf:
    zf.writestr("dir/file.txt", "This is a test")

# Reset the BytesIO object's cursor to the start.
zip_buffer.seek(0)

with zipfile.ZipFile(zip_buffer, "r") as zf:
    zpath = zipfile.Path(zf, "/")

dir_name, file_name = zpath.at.split("/")
```

```
> ruff --select=PD008 a.py
a.py:24:23: PD008 Use `.loc` instead of `.at`.  If speed is important, use numpy.
   |
24 | dir_name, file_name = zpath.at.split("/")
   |                       ^^^^^^^^ PD008
   |

Found 1 error.
```

---

_Label `bug` added by @charliermarsh on 2023-05-17 13:23_

---

_Comment by @charliermarsh on 2023-05-17 13:23_

Thanks! Some of the `PD` false positives are hard to avoid, but this one should be fixable.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-17 15:28_

---

_Closed by @charliermarsh on 2023-05-17 16:21_

---
