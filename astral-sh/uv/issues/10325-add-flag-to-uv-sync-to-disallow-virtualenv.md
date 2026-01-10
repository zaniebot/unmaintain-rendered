---
number: 10325
title: "Add flag to `uv sync` to disallow virtualenv recreation"
type: issue
state: open
author: sd2k
labels:
  - configuration
  - needs-design
  - cli
assignees: []
created_at: 2025-01-06T11:13:21Z
updated_at: 2025-11-09T15:11:07Z
url: https://github.com/astral-sh/uv/issues/10325
synced_at: 2026-01-10T01:24:52Z
---

# Add flag to `uv sync` to disallow virtualenv recreation

---

_Issue opened by @sd2k on 2025-01-06 11:13_

I've been porting a few Dockerfiles from Poetry to uv using a multistage build and the following happened:

1. install build dependencies into the `builder` stage
2. run `uv sync` with pyproject.toml/uv.lock mounted which works fine
3. copy code into the final stage
4. copy virtualenv from the final stage
5. run `uv sync` again; it ends up trying to rebuild dependencies, which fails this time since system deps aren't available. Turning on verbose logging shows 'Removing virtualenv...' and 'Creating virtualenv...' because the Python version differed.

This ended up being because there was a .python-version in the code directory which was seen by the second `uv sync` run and triggered a recreation of the venv. I can fix it by removing the .python-version (or equivalently including it in the mounts in stage 2, but I only actually noticed it because I had dependencies with build requirements; otherwise, my second stage would have been quietly recreating the virtualenv every time and it'd be hard to notice.

I think it would be nice to have a flag on `uv sync` which hard fails if the current virtualenv doesn't match the one preferred by the project, rather than implicitly recreating. This is especially important if the initial virtualenv was created using flags like `--relocatable`, which (I assume?) aren't replicated by the second virtualenv created by `uv sync`.

Happy to contribute if this is something you'd accept!

---

_Comment by @sd2k on 2025-01-06 11:14_

This seems similar to https://github.com/astral-sh/uv/issues/1472 but refers to a different subcommand.

---

_Label `configuration` added by @zanieb on 2025-01-06 20:26_

---

_Label `needs-design` added by @zanieb on 2025-01-06 20:26_

---

_Label `cli` added by @zanieb on 2025-01-06 20:26_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-06-26 09:49_

---

_Unassigned @jtfmumm by @zanieb on 2025-11-09 15:11_

---
