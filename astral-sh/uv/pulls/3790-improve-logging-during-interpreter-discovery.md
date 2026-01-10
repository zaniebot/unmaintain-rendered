```yaml
number: 3790
title: Improve logging during interpreter discovery
type: pull_request
state: merged
author: zanieb
labels:
  - tracing
assignees: []
merged: true
base: main
head: zb/interp-log
created_at: 2024-05-23T13:26:19Z
updated_at: 2024-05-23T15:25:23Z
url: https://github.com/astral-sh/uv/pull/3790
synced_at: 2026-01-10T14:32:20Z
```

# Improve logging during interpreter discovery

---

_Pull request opened by @zanieb on 2024-05-23 13:26_

```
❯ VIRTUAL_ENV=.venv cargo run -q -- pip install anyio -v
DEBUG Searching for interpreter that fulfills Python @ default in virtual environments
DEBUG Found Python interpreter cpython 3.12.3 at `.venv/bin/python` (active virtual environment)
DEBUG Using Python 3.12.3 environment at .venv/bin/python
DEBUG Trying to lock if free: .venv/.lock
DEBUG Requirement satisfied: anyio
DEBUG Requirement satisfied: idna>=2.8
DEBUG Requirement satisfied: sniffio>=1.1
DEBUG All editables satisfied: 
Audited 1 package in 14ms
```

instead of

```
❯ VIRTUAL_ENV=.venv cargo run -q -- pip install anyio -v
DEBUG Searching for interpreter that fulfills Python @ default
DEBUG Searching for interpreter that fulfills Python @ default
INFO Found active virtual environment (via VIRTUAL_ENV) at: .venv
DEBUG Found a virtual environment at: /Users/zb/workspace/uv/.venv
DEBUG Using Python 3.12.3 environment at .venv/bin/python
DEBUG Trying to lock if free: .venv/.lock
DEBUG Requirement satisfied: anyio
DEBUG Requirement satisfied: idna>=2.8
DEBUG Requirement satisfied: sniffio>=1.1
DEBUG All editables satisfied: 
Audited 1 package in 16ms
```

---

_Label `tracing` added by @zanieb on 2024-05-23 13:26_

---

_Comment by @charliermarsh on 2024-05-23 14:56_

What is `Python @ default`?

Should `Found Python interpreter cpython 3.12.3` perhaps be something like `Found cpython@3.12.3 at...`? "Python interpreter {impl}" is maybe a bit redundant?

---

_Comment by @zanieb on 2024-05-23 15:00_

"Python @ default" was resolved / removed in #3789 

The implementation name display is being changed in https://github.com/astral-sh/uv/pull/3791 — I do agree it feels a little redundant though. Let me rebase onto `main` and tweak a bit more.

---

_Comment by @zanieb on 2024-05-23 15:03_

Now it looks like


```
DEBUG Searching for interpreter that fulfills any Python in virtual environments
DEBUG Found CPython 3.12.3 at `.venv/bin/python` (active virtual environment)
DEBUG Using Python 3.12.3 environment at .venv/bin/python
```

---

_Comment by @zanieb on 2024-05-23 15:10_

Specialized the search message per request kind so we can do a little better e.g.

```
❯ VIRTUAL_ENV=.venv cargo run -q -- pip install anyio -v
DEBUG Searching for Python interpreter in virtual environments
DEBUG Found CPython 3.12.3 at `.venv/bin/python` (active virtual environment)
DEBUG Using Python 3.12.3 environment at .venv/bin/python
```

```
❯ cargo run -q -- pip install anyio -v --python cpython
DEBUG Searching for a CPython interpreter in all sources
DEBUG Found CPython 3.12.3 at `/Users/zb/workspace/uv/.venv/bin/python` (virtual environment)
DEBUG Using Python 3.12.3 environment at .venv/bin/python
```

```
❯ cargo run -q -- pip install anyio -v --python .venv
DEBUG Checking for Python interpreter in directory .venv
DEBUG Using Python 3.12.3 environment at .venv/bin/python
```

```
❯ cargo run -q -- pip install anyio -v --python .venv/bin/python
DEBUG Checking for Python interpreter at path .venv/bin/python
DEBUG Using Python 3.12.3 environment at .venv/bin/python
```

---

_Merged by @zanieb on 2024-05-23 15:25_

---

_Closed by @zanieb on 2024-05-23 15:25_

---

_Branch deleted on 2024-05-23 15:25_

---
