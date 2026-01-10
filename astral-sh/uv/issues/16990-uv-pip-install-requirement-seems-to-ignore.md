---
number: 16990
title: "`uv pip install --requirement` seems to ignore pyproject.toml `tool.uv.sources` for explicit indexes"
type: issue
state: closed
author: pachoo
labels:
  - question
assignees: []
created_at: 2025-12-04T20:20:46Z
updated_at: 2025-12-09T13:51:43Z
url: https://github.com/astral-sh/uv/issues/16990
synced_at: 2026-01-10T01:26:12Z
---

# `uv pip install --requirement` seems to ignore pyproject.toml `tool.uv.sources` for explicit indexes

---

_Issue opened by @pachoo on 2025-12-04 20:20_

### Summary

When installing a requirements file in the same directory as a pyproject.toml file it seems that the pyproject.toml's `tool.uv.sources` is ignored when that index is marked explicit.

uv version: 0.9.15

I would expect this to work since although the index is marked as explicit, it is identified as the index to use in `tool.uv.sources`.  When the index is not marked explicit, the requirement is found.


Example pyproject.toml
```
[project]
dependencies = [
  'public_foo',
  'private_bar',
]

[tool.uv.sources]
private_bar = { index = "private-repo" }

[[tool.uv.index]]
name = "private-repo"
url = "https://my-private-repo.company.com"
explicit = true
```

example requirements.txt
```
public_foo==1.2.3
private_bar==2.3.4
```

Usage
```
UV_INDEX_PRIVATE_REPO_USERNAME=... UV_INDEX_PRIVATE_REPO_PASSWORD=... uv pip install -r requirements.txt
```

### Platform

macOS 15.6.1 arm64

### Version

uv 0.9.15 (5eafae332 2025-12-02)

### Python version

Python 3.12.10

---

_Label `bug` added by @pachoo on 2025-12-04 20:20_

---

_Comment by @jreiml on 2025-12-04 22:32_

There are also other cases where this is ignored: https://github.com/astral-sh/uv/issues/16933#issuecomment-3608891530

I think `tool.uv.sources` might be ignored in general for `uv pip install` (?).

---

_Comment by @charliermarsh on 2025-12-05 03:44_

Sources apply to dependencies defined in the project (i.e., the `pyproject.toml`); they don't apply to a `requirements.txt` file.

---

_Label `bug` removed by @charliermarsh on 2025-12-05 03:44_

---

_Label `question` added by @charliermarsh on 2025-12-05 03:44_

---

_Comment by @pachoo on 2025-12-05 15:17_

@charliermarsh thanks for the clarification and update of the label.   If this is the expected behavior (which it sounds like it is) is the recommendation to use `uv lock` and `uv sync` to achieve similar ends, pinned requirements that will "honor" the explicit tag for the index?   If this is something that could change in the future, I'm happy to make an enhancement as well.



---

_Comment by @charliermarsh on 2025-12-05 19:04_

I'd recommend:
```toml
[project]
dependencies = [
  "public_foo==1.2.3",
  "private_bar==2.3.4",
]

[tool.uv.sources]
private_bar = { index = "private-repo" }

[[tool.uv.index]]
name = "private-repo"
url = "https://my-private-repo.company.com"
explicit = true
```

---

_Comment by @pachoo on 2025-12-09 13:51_

thanks @charliermarsh.  Since it sounds like uv is intentionally not processing `uv.sources` for requirements, I'll close this issue.


---

_Closed by @pachoo on 2025-12-09 13:51_

---
