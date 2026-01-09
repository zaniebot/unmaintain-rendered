---
number: 9554
title: uvx ignores index from uv.toml and pyproject.toml
type: issue
state: open
author: zdxerr
labels:
  - needs-decision
assignees: []
created_at: 2024-12-01T14:42:15Z
updated_at: 2025-04-01T19:00:53Z
url: https://github.com/astral-sh/uv/issues/9554
synced_at: 2026-01-07T13:12:18-06:00
---

# uvx ignores index from uv.toml and pyproject.toml

---

_Issue opened by @zdxerr on 2024-12-01 14:42_

If I run a tool with uvx it does not automatically discover the uv.toml file in the cwd. I have to run it like `uvx --config-file uv.toml ruff check`.

The issue was present with uv 0.5.4 and 0.5.5.

---

_Comment by @charliermarsh on 2024-12-01 14:49_

This is a duplicate of https://github.com/astral-sh/uv/issues/8299.

---

_Closed by @charliermarsh on 2024-12-01 14:49_

---

_Reopened by @charliermarsh on 2024-12-01 14:49_

---

_Comment by @charliermarsh on 2024-12-01 14:49_

Wait wrong issue -- one sec.

---

_Comment by @charliermarsh on 2024-12-01 14:52_

This is intentional. `uvx` (and `uv tool`) are global commands, so you should configure them with the user-level or system-level `uv.toml`. They intentionally don't read configuration from the current working directory.

---

_Label `question` added by @charliermarsh on 2024-12-01 14:52_

---

_Comment by @zanieb on 2024-12-03 15:54_

I actually think I would expect `uvx` / `uv tool run` to discover `uv.toml` in the working directory (but not a `pyproject.toml`)? It's a little confusing because I _wouldn't_ expect `uv tool install` to do so, though I think I could go either way there.

---

_Label `needs-decision` added by @zanieb on 2025-01-07 19:30_

---

_Comment by @Zoltag on 2025-03-14 09:40_

@charliermarsh - I've found that when calling uvx inside a project like `uvx --with .[test] --verbose coverage run -m pytest`, it correctly reads the local pyproject file to determine the dependencies, but it ignores the defined indices, so dependencies not hosted on public pypi cannot be installed

I appreciate this is not the intended use-case for uvx, but this seems counter-intuitive to me?

---

_Comment by @sanmai-NL on 2025-03-26 08:48_

@charliermarsh The previous comment seems like a legit defect. I like the idea to make a distinction between `uvx` and `uv tool` in behavior wrt. project (product) configuration along the lines of @zanieb's thoughts.

---

_Label `question` removed by @zanieb on 2025-04-01 19:00_

---
