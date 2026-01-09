---
number: 6376
title: "importlib.metadata.distributions() doesn't surface all installed packages/endpoints, breaking pytest plugins"
type: issue
state: closed
author: ashbywinch
labels: []
assignees: []
created_at: 2024-08-21T20:47:16Z
updated_at: 2024-08-22T19:58:01Z
url: https://github.com/astral-sh/uv/issues/6376
synced_at: 2026-01-07T13:12:17-06:00
---

# importlib.metadata.distributions() doesn't surface all installed packages/endpoints, breaking pytest plugins

---

_Issue opened by @ashbywinch on 2024-08-21 20:47_

I tried to migrate a project to uv, but pytest could no longer find its plugins pytest-cov and pytest-testmon.

    $ uvx pytest --testmon
    ERROR: usage: pytest [options] [file_or_dir] [file_or_dir] [...]
    pytest: error: unrecognized arguments: --testmon
      
On investigation it seems that pytest uses Pluggy to load the plugins, and Pluggy uses importlib.metadata.distributions() to identify and load installed packages that have entrypoints matching its naming convention (see [Pluggy source](https://github.com/pytest-dev/pluggy/blob/82d4e1ce62e11a76b9d050e457adc41c4c69f7db/src/pluggy/_manager.py#L376C9-L376C36))

I made an empty project with "uv init", added dependencies [ "pytest", "pytest-cov", "pytest-testmon" ], ran "uv sync" successfully (I can see the installed files in .venv/lib/site-packages) and ran the following code

    for dist in list(importlib.metadata.distributions()):
        print("name: ", dist.name)
        for ep in dist.entry_points:
             print("ep: ", ep.name, ep.group)

Under my regular project this code prints (among other things):

    name:  pytest-cov
    ep:  pytest_cov pytest11
    name:  pytest-testmon
    ep:  pytest-testmon pytest11

Under my uv test project those modules/endpoints are not in the list, even though they have been successfully installed.

    $ uv --version
    uv 0.3.0 (dd1934c9c 2024-08-20)

and this is on Win11.


---

_Comment by @charliermarsh on 2024-08-21 20:48_

I think the problem you're running into here is that `uvx` runs in an environment isolated from your project. Can you try `uv run pytest` instead? Or even `uv run --with pytest pytest` if it's not part of your project environment?

---

_Comment by @zanieb on 2024-08-21 20:59_

Some documentation on this at https://docs.astral.sh/uv/concepts/tools/#relationship-to-uv-run

We may be able to improve the documentation though, if you have any questions.

---

_Referenced in [astral-sh/uv#6377](../../astral-sh/uv/issues/6377.md) on 2024-08-21 21:00_

---

_Comment by @ashbywinch on 2024-08-22 13:35_

Yes, that's fixed it! Thanks 👍 
I think it might have helped if this page here: https://docs.astral.sh/uv/guides/tools/  had a section right at the top clarifying why one would or wouldn't want to run a tool without installing, and pointing to the section on how to run a tool WITH installing (or without installing but with plugins, which is mentioned, but much further down the page). 
The info I needed was all there, but I didn't realise it would be relevant when I first read it, and I didn't know where to look for solutions once I ran into the problem.

---

_Comment by @zanieb on 2024-08-22 18:29_

Addressing that with https://github.com/astral-sh/uv/pull/6454 — let me know if that's not helpful.

---

_Closed by @zanieb on 2024-08-22 18:29_

---

_Comment by @ashbywinch on 2024-08-22 19:57_

That looks spot on 👍

---
