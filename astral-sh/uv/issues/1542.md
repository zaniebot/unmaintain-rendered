```yaml
number: 1542
title: "`uv` does not understand URL-encoded link data"
type: issue
state: closed
author: SnoopJ
labels:
  - bug
assignees: []
created_at: 2024-02-16T21:06:02Z
updated_at: 2024-02-16T21:47:05Z
url: https://github.com/astral-sh/uv/issues/1542
synced_at: 2026-01-10T05:40:31Z
```

# `uv` does not understand URL-encoded link data

---

_Issue opened by @SnoopJ on 2024-02-16 21:06_

`uv` commands offer `--find-links`, but do not behave as `pip` and `pip-compile` do when dealing with indices that URL-encode their package names/URLs. For reference, see the [implementation of the correct behavior in `pip`](https://github.com/pypa/pip/blob/51de88ca6459fdd5213f86a54b021a80884572f9/src/pip/_internal/models/link.py#L382-L394).

Without this support, packages provided by PyTorch that explicitly identify their CUDA support cannot be installed with `uv pip install` or resolved by `uv pip compile`.

## Example reproduction

```
$ uv pip install --find-links https://download.pytorch.org/whl/torch_stable.html "torch==2.0.0+cu118"
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of torch==2.0.0+cu118 and you require torch==2.0.0+cu118, we can conclude that the requirements are
      unsatisfiable.
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-16 21:19_

---

_Label `bug` added by @charliermarsh on 2024-02-16 21:19_

---

_Comment by @charliermarsh on 2024-02-16 21:19_

Thanks! I reproduced, I'll fix this now.

---

_Closed by @charliermarsh on 2024-02-16 21:47_

---

_Closed by @charliermarsh on 2024-02-16 21:47_

---
