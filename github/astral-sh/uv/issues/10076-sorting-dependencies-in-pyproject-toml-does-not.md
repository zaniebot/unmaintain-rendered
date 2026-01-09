---
number: 10076
title: "Sorting dependencies in `pyproject.toml` does not work as expected"
type: issue
state: closed
author: paduszyk
labels:
  - bug
assignees: []
created_at: 2024-12-21T13:26:05Z
updated_at: 2025-01-18T15:25:59Z
url: https://github.com/astral-sh/uv/issues/10076
synced_at: 2026-01-07T13:12:18-06:00
---

# Sorting dependencies in `pyproject.toml` does not work as expected

---

_Issue opened by @paduszyk on 2024-12-21 13:26_

### Related issues

Relates to #6203.

### Setup

- uv 0.5.11 (Homebrew 2024-12-20)
- Python 3.13
- macOS Sequoia 15.1.1

### Observed behavior

When adding a series of dependencies with `uv add`:

```bash
uv add ruff
uv add pytest
uv add pytest-mock
uv add pytest-randomly
```

this is what I get in my `pyproject.toml`:

```toml
[project]
# ...
dependencies = [
    "pytest-mock>=3.14.0",
    "pytest>=8.3.4",
    "ruff>=0.8.4",
    "pytest-randomly>=3.16.0",
]
```

The list of dependencies is updated as follows:

1. `uv add ruff`: `ruff` is added.
2. `uv add pytest`: `pytest` is inserted before `ruff`.
3. `uv add pytest-mock`: `pytest-mock` is inserted before `pytest`.
4. `uv add pytest-randomly`: `pytest-randomly` is appended as the last item.

So, sorting is broken at point 3.

### Expected behavior

Since uv _should_ sort the dependencies alphabetically (see #6388), I would rather expect:

```toml
[project]
# ...
dependencies = [
    "pytest>=8.3.4",
    "pytest-mock>=3.14.0",
    "pytest-randomly>=3.16.0",
    "ruff>=0.8.4",
]
```

or, assuming that `>=` are taken into account in sorting:

```toml
[project]
# ...
dependencies = [
    "pytest-mock>=3.14.0",
    "pytest-randomly>=3.16.0",
    "pytest>=8.3.4",
    "ruff>=0.8.4",
]
```

I believe that the first options seems to be more intuitive. In fact, by "sorting" dependencies I would rather think of sorting dependency names, not its specifiers.

---

_Label `bug` added by @charliermarsh on 2024-12-21 13:38_

---

_Comment by @charliermarsh on 2024-12-21 13:38_

Thanks! Looks like a bug.

---

_Referenced in [astral-sh/uv#10078](../../astral-sh/uv/pulls/10078.md) on 2024-12-21 13:55_

---

_Closed by @charliermarsh on 2024-12-21 14:43_

---

_Comment by @Kavan72 on 2025-01-18 08:48_

@charliermarsh I’m encountering a strange bug. When I try to add a new library to an existing project, sorting doesn’t work properly. However, when I create a new project, it works perfectly. I’m using the latest version of UV (0.5.21). 

---

_Comment by @charliermarsh on 2025-01-18 15:25_

@Kavan72 -- Please open a new issue with a clear, minimal reproduction. Thanks.

---

_Referenced in [astral-sh/uv#10738](../../astral-sh/uv/issues/10738.md) on 2025-01-18 19:04_

---
