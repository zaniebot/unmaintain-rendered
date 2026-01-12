```yaml
number: 15932
title: Always ensure wheels are compatible with the required Python versions
type: pull_request
state: open
author: zanieb
labels:
  - bug
assignees: []
draft: true
base: main
head: zb/python-markers-implied
created_at: 2025-09-18T14:04:11Z
updated_at: 2025-09-18T14:21:52Z
url: https://github.com/astral-sh/uv/pull/15932
synced_at: 2026-01-12T16:12:01Z
```

# Always ensure wheels are compatible with the required Python versions

---

_@zanieb_

Following up on https://github.com/astral-sh/uv/issues/14836 and #14913

```
❯ uv init example -p 3.8 && cd example
❯ uv add torch --exclude-newer 2025-05-01
Using CPython 3.8.20
Creating virtual environment at: .venv
Resolved 46 packages in 659ms
error: Distribution `torch==2.5.1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're using CPython 3.8 (`cp38`), but `torch` (v2.5.1) only has wheels with the following Python implementation tags: `cp39`, `cp310`, `cp311`, `cp312`, `cp313`
❯ cargo run -q -- add torch --exclude-newer 2025-05-01
Resolved 45 packages in 69ms
Installed 9 packages in 247ms
 + filelock==3.16.1
 + fsspec==2025.3.0
 + jinja2==3.1.6
 + markupsafe==2.1.5
 + mpmath==1.3.0
 + networkx==3.1
 + sympy==1.13.3
 + torch==2.4.1
 + typing-extensions==4.13.2
```

---

_Label `bug` added by @zanieb on 2025-09-18 14:04_

---

_Comment by @zanieb on 2025-09-18 14:11_

This is a fairly naive split of the previous implementaiton.

This includes forking on Python implementation markers — we might want to just do Python versions?

---

_Comment by @zanieb on 2025-09-18 14:21_

Interesting! I broke some integration tests

---
