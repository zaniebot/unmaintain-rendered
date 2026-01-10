```yaml
number: 9758
title: "uv add deletes 'root' comments from within the dependency-group section of pyproject.toml"
type: issue
state: closed
author: dean-tate
labels:
  - bug
  - external
assignees: []
created_at: 2024-12-10T00:43:46Z
updated_at: 2024-12-10T19:59:15Z
url: https://github.com/astral-sh/uv/issues/9758
synced_at: 2026-01-10T04:36:21Z
```

# uv add deletes 'root' comments from within the dependency-group section of pyproject.toml

---

_Issue opened by @dean-tate on 2024-12-10 00:43_

uv version: 0.5.7


```
[project]
name = "myproject"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.11"
[dependency-groups]
# These are our dev dependencies
dev = [
        "black",
]
# These are our test dependencies
test = [
        "pytest",
        "pytest-cov",
]
```
 
[tool.uv]
default-groups = ["dev", "test"]


Running `uv add --group dev isort` adds the isort dependency to the correct group, but all of the comment lines within the [dependency-groups] section are removed.

```
[project]
name = "myproject"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.11"
[dependency-groups]
dev = [
        "black",
        "isort>=5.13.2",
]
test = [
        "pytest",
        "pytest-cov",
]
 
[tool.uv]
default-groups = ["dev", "test"]
```


Interestingly this doesn't occur for uv remove. 

There's another ticket about an issue with comments as well - could be worth grouping these. 

https://github.com/astral-sh/uv/issues/8982. 

---

_Renamed from "uv add deletes `root` comments from within the dependency-group section of pyproject.toml" to "uv add deletes 'root' comments from within the dependency-group section of pyproject.toml" by @dean-tate on 2024-12-10 00:45_

---

_Comment by @charliermarsh on 2024-12-10 01:16_

Thanks, that's surprising!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-10 01:17_

---

_Label `bug` added by @charliermarsh on 2024-12-10 01:17_

---

_Label `upstream` added by @charliermarsh on 2024-12-10 01:39_

---

_Comment by @charliermarsh on 2024-12-10 01:39_

It looks like this is coming from `toml-edit`: https://github.com/toml-rs/toml/issues/818.

---

_Closed by @charliermarsh on 2024-12-10 19:59_

---

_Closed by @charliermarsh on 2024-12-10 19:59_

---
