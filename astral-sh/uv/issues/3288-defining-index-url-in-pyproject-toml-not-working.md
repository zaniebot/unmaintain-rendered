```yaml
number: 3288
title: Defining index-url in pyproject.toml not working
type: issue
state: closed
author: florianfischer91
labels:
  - bug
assignees: []
created_at: 2024-04-27T09:49:21Z
updated_at: 2024-04-27T12:52:08Z
url: https://github.com/astral-sh/uv/issues/3288
synced_at: 2026-01-12T15:58:42Z
```

# Defining index-url in pyproject.toml not working

---

_@florianfischer91_

First of all thanks for developing uv (and ruff). They are really amazing :-)
 
I was trying to set the `index-url` in a `pyproject.toml`, but im always getting the following error:

```
error: Failed to parse `pyproject.toml`
  Caused by: TOML parse error at line 2, column 5
  |
2 | Url="https://randomstring/simple"
  |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
invalid type: string "https://randomstring/simple", expected struct VerbatimUrl
```

Example to reproduce the bug, call `UV_CONFIG_FILE=pyproject.toml uv pip show ruff` where `pyproject.toml` contains

```
[tool.uv.pip.index-url]
Url="https://randomstring/simple"
```

uv version: 0.1.38

Am I doing something wrong or is this still not ready for use?

---

_Comment by @charliermarsh on 2024-04-27 11:46_

I think you would want:

```toml
[tool.uv.pip]
index-url = "https://randomstring/simple"
```


---

_Label `question` added by @charliermarsh on 2024-04-27 11:46_

---

_Comment by @florianfischer91 on 2024-04-27 12:35_

I have already tried that but i also results in an error:
```
error: Failed to parse `pyproject.toml`
  Caused by: TOML parse error at line 2, column 13
  |
2 | index-url = "https://randomstring/simple"
  |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
unknown variant `https://randomstring/simple`, expected one of `Pypi`, `Url`, `Path`
```

---

_Comment by @charliermarsh on 2024-04-27 12:36_

Oh sorry, thanks, I'll fix that.

---

_Label `question` removed by @charliermarsh on 2024-04-27 12:36_

---

_Label `bug` added by @charliermarsh on 2024-04-27 12:36_

---

_Comment by @charliermarsh on 2024-04-27 12:36_

(This isn't an officially supported feature yet, it's undocumented.)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-27 12:37_

---

_Comment by @charliermarsh on 2024-04-27 12:43_

Please continue to file bugs on it though, much appreciated!

---

_Comment by @charliermarsh on 2024-04-27 12:43_

Fixed in https://github.com/astral-sh/uv/pull/3289.

---

_Closed by @charliermarsh on 2024-04-27 12:52_

---
