---
number: 12032
title: "Can't install kaleido on Mac"
type: issue
state: open
author: uuip
labels:
  - question
assignees: []
created_at: 2025-03-07T05:12:57Z
updated_at: 2025-04-17T16:05:42Z
url: https://github.com/astral-sh/uv/issues/12032
synced_at: 2026-01-07T13:12:18-06:00
---

# Can't install kaleido on Mac

---

_Issue opened by @uuip on 2025-03-07 05:12_

### Summary

```
uv add 'vanna[postgres,mysql,clickhouse,duckdb,oracle]==0.7.6'
Resolved 381 packages in 4.03s
error: Distribution `kaleido==0.2.1.post1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on macOS (`macosx_15_0_arm64`), but `kaleido` (v0.2.1.post1) only has wheels for the following platform: `manylinux2014_armv7l`
```

```toml
[project]
name = "vanna"
version = "0.7.6"
authors = [
  { name="Zain Hoda", email="zain@vanna.ai" },
]

description = "Generate SQL queries from natural language"
readme = "README.md"
requires-python = ">=3.9"
classifiers = [
    "Programming Language :: Python :: 3",
    "License :: OSI Approved :: MIT License",
    "Operating System :: OS Independent",
]
dependencies = [
    "requests", "tabulate", "plotly", "pandas", "sqlparse", "kaleido", "flask", "flask-sock", "flasgger", "sqlalchemy"
]
```
You can install it directly using pip, and the installed version will be kaleido==0.2.1, not 0.2.1.post1.

### Platform

macOS 15

### Version

uv 0.6.4 (Homebrew 2025-03-03)

### Python version

3.13.2

---

_Label `bug` added by @uuip on 2025-03-07 05:12_

---

_Comment by @charliermarsh on 2025-03-07 17:05_

This is mostly a problem with `kaleido`. You can read about this problem in detail here: https://docs.astral.sh/uv/concepts/resolution/#required-environments.

The short answer is that I'd suggest adding a constraint:

```toml
[tool.uv]
constraint-dependencies = ["kaleido==0.2.1"]
```

Alternatively you can set a required environment:
```toml
[tool.uv]
required-environments = ["sys_platform == 'darwin'"] 
```

Though you'll continue to see weird things on other Linux platforms since they _only_ publish an ARM Linux wheel.

---

_Label `bug` removed by @charliermarsh on 2025-03-07 17:05_

---

_Label `question` added by @charliermarsh on 2025-03-07 17:05_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-07 17:05_

---

_Comment by @v479038280 on 2025-04-17 16:04_

But using uv pip install vanna can be installed normally, however, after that, when uv sync is executed, it will still report that kaleido cannot be installed,This also occurs on windows

---
