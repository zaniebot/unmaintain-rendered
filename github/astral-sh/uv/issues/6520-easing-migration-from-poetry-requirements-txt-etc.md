---
number: 6520
title: Easing migration from Poetry, requirements.txt, etc.
type: issue
state: closed
author: raayu83
labels: []
assignees: []
created_at: 2024-08-23T14:41:15Z
updated_at: 2024-08-26T07:35:01Z
url: https://github.com/astral-sh/uv/issues/6520
synced_at: 2026-01-07T13:12:17-06:00
---

# Easing migration from Poetry, requirements.txt, etc.

---

_Issue opened by @raayu83 on 2024-08-23 14:41_

First off, congrats on the 0.3.x Release!
I've already started migrating my projects to it!

Right now, if you have an existing project and want to migrate to uv, you have to add all settings an dependencies manually.
I assume adoption of uv could be increased if it was easier to get started.

Therefore it would be great if there was an `uv init --from <source>` command, e.g. `uv init --from poetry` or `uv init --from requirements`.
This option would import the dependencies and any other fields that can be converted from the corresponding configuration source. e.g. pyproject.toml for poetry.

Since poetry uses different version specifiers, this would also require a resolver to convert poetry version specifications into uv version specifications.

Whether it would be a good idea to automatically remove the original poetry definitions from pyproject.toml after conversion or not, I'm not sure. There are pros and cons.

Additionally, since there might already be a pypoject.toml e.g. when importing poetry dependencies, if the pyproject.toml already exists it would simply be updated instead of being created from scratch.


---

_Comment by @charliermarsh on 2024-08-23 14:52_

Totally agree... I actually have this working on a branch and it's kind of magical. You can do `uv add -r /path/to/pyproject.toml` and it translates the Poetry dependency specifiers (but not the configuration, which would be harder). We've just been stuck on figuring out the right API to expose to users.

---

_Comment by @charliermarsh on 2024-08-23 14:54_

See: https://github.com/astral-sh/uv/issues/6277 / https://github.com/astral-sh/uv/pull/6374

(And also https://github.com/astral-sh/uv/issues/6164)

---

_Comment by @zanieb on 2024-08-23 15:05_

And also #5200 

---

_Comment by @charliermarsh on 2024-08-23 15:05_

Gonna close given the other issues.

---

_Closed by @charliermarsh on 2024-08-23 15:05_

---

_Referenced in [astral-sh/uv#1804](../../astral-sh/uv/issues/1804.md) on 2024-08-23 15:27_

---

_Comment by @raayu83 on 2024-08-26 07:35_

So it was already planned all along, thats great!

---
