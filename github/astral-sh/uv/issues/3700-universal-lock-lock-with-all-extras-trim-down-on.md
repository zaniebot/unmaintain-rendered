---
number: 3700
title: "universal-lock: Lock with all extras, trim down on installation"
type: issue
state: closed
author: konstin
labels:
  - preview
assignees: []
created_at: 2024-05-21T15:34:17Z
updated_at: 2024-05-29T19:25:59Z
url: https://github.com/astral-sh/uv/issues/3700
synced_at: 2026-01-07T13:12:17-06:00
---

# universal-lock: Lock with all extras, trim down on installation

---

_Issue opened by @konstin on 2024-05-21 15:34_

The lockfile needs to be built on a resolution with all extras of the root project and workspace requirements, but when installing we only want the extras and workspace packages the user requested. The need to trim the resolution, probably by a DAG traversal. This includes mapping unnamed requirements to their named counterpart in the resolution map.

---

_Label `preview` added by @konstin on 2024-05-21 15:34_

---

_Comment by @BurntSushi on 2024-05-21 15:37_

> The lockfile needs to be built on a resolution with all extras and workspace requirements

I think there is a hitch here right? Because some packages will have "dev" extras, and we specifically don't want to bring those into the lock file right? Basically that a package shouldn't inherit the dev dependencies of its dependencies. How do we deal with that?

---

_Comment by @zanieb on 2024-05-21 15:44_

Development dependencies are separate from extras, technically. They can be placed in an extra, but generally they are separate and not a part of the standard metadata (i.e. it's project management tool specific).

---

_Comment by @zanieb on 2024-05-21 15:44_

There's a bigger problem here though... extras can conflict with each other. Are we going to solve with them all turned on and ban conflicts? Are we going to lock all combinations?

---

_Comment by @konstin on 2024-05-21 15:49_

If we want to support conflicting extras, the user needs to specify which those are and we apply the same split-resolve-merge as for conflicting disjoint markers. The ensures that extras are generally unified and conflict free and avoids combinatorial explosion.

---

_Comment by @zanieb on 2024-05-21 16:04_

Do we know how they're going to express the conflict? Can you just do it with `<self-name>[extra]` markers?

---

_Comment by @konstin on 2024-05-21 16:09_

We need additional `tool.uv` metadata for this, i don't think any other tool covers this yet. I've heard that some large complex frameworks had trouble adopting poetry due to this.

---

_Comment by @charliermarsh on 2024-05-21 22:21_

I can take this if we want to start by "including all extras, then trimming at install time".

---

_Referenced in [astral-sh/uv#3705](../../astral-sh/uv/pulls/3705.md) on 2024-05-22 12:29_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-22 13:18_

---

_Comment by @olivier-lacroix on 2024-05-24 15:22_

You may want to take inspiration from pixi, which handles this via features (equivalent to a group of extra), that are included in environment(s).
Environments are included in the lock-file, and sometimes solved together when requested.
This avoids combinatorial explosion and is pretty flexible.

---

_Referenced in [astral-sh/uv#3404](../../astral-sh/uv/issues/3404.md) on 2024-05-26 18:55_

---

_Referenced in [astral-sh/uv#3913](../../astral-sh/uv/pulls/3913.md) on 2024-05-29 17:15_

---

_Closed by @charliermarsh on 2024-05-29 19:26_

---
