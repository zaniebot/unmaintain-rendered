```yaml
number: 9693
title: "`uv` incorrectly (?) installs `dbt-clickhouse`"
type: issue
state: closed
author: ivanychev
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-12-06T18:56:05Z
updated_at: 2024-12-08T04:17:25Z
url: https://github.com/astral-sh/uv/issues/9693
synced_at: 2026-01-10T04:36:21Z
```

# `uv` incorrectly (?) installs `dbt-clickhouse`

---

_Issue opened by @ivanychev on 2024-12-06 18:56_

When installing `dbt-core` and `dbt-clickhouse`, dbt can't see ClickHouse plugin
 
* Platform: M2 Pro, macOS 15.1.1
* `uv` version: 0.5.6

# Steps to reproduce

pyproject.toml:

```toml
[project]
name = "dbt-clickhouse"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = "==3.11.9"
dependencies = [
    "dbt-clickhouse==1.8.5",
    "dbt-core==1.8.9"
]
```

```
uv lock && uv sync
uv run dbt --version
```

outputs

```
Core:
  - installed: 1.8.9
  - latest:    1.8.9 - Up to date!

Plugins:


```

# Expected behavior

Using `pyenv`:

```
pyenv shell 3.11.9
python -m venv venv
source venv/bin/activate
pip install dbt-core==1.8.9 dbt-clickhouse==1.8.5
dbt --version
```

outputs

```
Core:
  - installed: 1.8.9
  - latest:    1.8.9 - Up to date!

Plugins:
  - clickhouse: 1.8.5 - Up to date!   # <<<<<<<<<<<<<<<<
```


---

_Comment by @charliermarsh on 2024-12-06 18:58_

It's because your project is called `dbt-clickhouse`.

---

_Comment by @charliermarsh on 2024-12-06 18:59_

I actually would've expected `uv lock` to fail, since you're asking for both `dbt-clickhouse` versions `0.1.0` and `1.8.5`.

---

_Label `bug` added by @charliermarsh on 2024-12-06 19:04_

---

_Comment by @charliermarsh on 2024-12-06 19:05_

You need to rename your project not to collide with the dependency, but this resolution should fail rather than succeed.

---

_Label `help wanted` added by @charliermarsh on 2024-12-06 19:05_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-07 14:07_

---

_Comment by @ivanychev on 2024-12-07 14:50_

Hey @charliermarsh ! Thanks, that's indeed my mistake. I think it would be great for `uv` to fail in this situation

---

_Closed by @charliermarsh on 2024-12-08 04:17_

---

_Closed by @charliermarsh on 2024-12-08 04:17_

---
