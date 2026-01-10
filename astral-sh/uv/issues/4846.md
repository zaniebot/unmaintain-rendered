```yaml
number: 4846
title: "`uv run --isolated` does not respect `--python` flag"
type: issue
state: closed
author: j178
labels:
  - bug
  - configuration
assignees: []
created_at: 2024-07-06T12:27:40Z
updated_at: 2024-07-06T19:22:20Z
url: https://github.com/astral-sh/uv/issues/4846
synced_at: 2026-01-10T05:31:37Z
```

# `uv run --isolated` does not respect `--python` flag

---

_Issue opened by @j178 on 2024-07-06 12:27_

```sh
$ python -V
Python 3.12.1

$ cargo run -q -- run --preview --isolated --python 3.12.3 python -V
3.12.1
// ^ should be 3.12.3

$ cargo run -q -- --version
uv 0.2.21 (ebfe6d8fc 2024-07-03)
```


---

_Renamed from "`uv run --isloated` does not respect `--python` flag" to "`uv run --isolated` does not respect `--python` flag" by @AlexWaygood on 2024-07-06 13:35_

---

_Label `configuration` added by @AlexWaygood on 2024-07-06 13:35_

---

_Label `bug` added by @charliermarsh on 2024-07-06 19:06_

---

_Comment by @charliermarsh on 2024-07-06 19:06_

Thanks, confirmed!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-06 19:06_

---

_Closed by @charliermarsh on 2024-07-06 19:22_

---
