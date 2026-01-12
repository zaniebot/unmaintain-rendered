```yaml
number: 6846
title: PTH122 makes an inaccurate (incomplete) recommendation
type: issue
state: closed
author: dbohdan
labels:
  - documentation
assignees: []
created_at: 2023-08-24T08:47:21Z
updated_at: 2023-08-25T22:11:57Z
url: https://github.com/astral-sh/ruff/issues/6846
synced_at: 2026-01-12T15:54:46Z
```

# PTH122 makes an inaccurate (incomplete) recommendation

---

_@dbohdan_

The message for [PTH122](https://beta.ruff.rs/docs/rules/os-path-splitext/) reads,

> `os.path.splitext()` should be replaced by `Path.suffix`

This is identical to the message for PL122 in [flake8-use-pathlib](https://gitlab.com/RoPP/flake8-use-pathlib). However, `os.path.splitext()` returns a tuple of `(root, ext)`  ([Python docs](https://docs.python.org/3/library/os.path.html#os.path.splitext)). This means that the replacement for `os.path.splitext()` can be `Path.suffix`, `Path.stem`, or both depending on which part of the tuple you use. 

I would update the message to something like

> `os.path.splitext()` should be replaced by `Path.stem` and/or `Path.suffix`

The value of `root` is more accurately `str(path.parent.joinpath(path.stem))` (that is, the stem plus the path). Here is code to demonstrate.

```
import os.path
from pathlib import Path

s = "/home/foo/bar.py"
root, _ = os.path.splitext(s)
path = Path(s)
stem_with_path = str(path.parent.joinpath(path.stem))
assert root == stem_with_path
print(root)  # Prints "/home/foo/bar".
```

The documentation for the rule can explain this. (There may be corner cases and platform-specific differences I am not aware of between `root` and `str(path.parent.joinpath(path.stem))`.)

I have [reported the issue](https://gitlab.com/RoPP/flake8-use-pathlib/-/issues/7) to flake8-use-pathlib, but it is not very active. It has had no code activity in a year.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-25 21:41_

---

_Comment by @charliermarsh on 2023-08-25 21:41_

Thanks for the thorough write-up. This makes sense and is easy to fix.

---

_Label `documentation` added by @charliermarsh on 2023-08-25 21:41_

---

_Closed by @charliermarsh on 2023-08-25 22:11_

---
