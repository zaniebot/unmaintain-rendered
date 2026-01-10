---
number: 10728
title: Support installing a tool from a subdirectory of a git repo
type: issue
state: closed
author: corneliusroemer
labels:
  - question
assignees: []
created_at: 2025-01-18T02:46:13Z
updated_at: 2025-09-25T10:37:10Z
url: https://github.com/astral-sh/uv/issues/10728
synced_at: 2026-01-10T01:24:57Z
---

# Support installing a tool from a subdirectory of a git repo

---

_Issue opened by @corneliusroemer on 2025-01-18 02:46_

I would like to install `vdsql` using `uvx`. Per [`vdsql` docs](https://github.com/saulpw/visidata/tree/70699c1c4edfdcb5d8a23a6554ea84c5bbc25ed4/visidata/apps/vdsql#or-install-manually-as-a-visidata-plugin-cutting-edge-development), one can install it by first cloning the repo and then changing to a subfolder of the repo as follows:

```
git clone git@github.com:saulpw/visidata.git
cd visidata/visidata/apps/vdsql
pip3 install .
```

I tried to install vdsql with `uvx` as follows, but this didn't work, as there seems to be no way to tell `uvx` to use a subfolder, not the root of the git repo:

```
$ uvx --from git+https://github.com/saulpw/visidata vdsql

 Updated https://github.com/saulpw/visidata (70699c1c)
The executable `vdsql` was not found.
warning: An executable named `vdsql` is not provided by package `visidata`.
The following executables are provided by `visidata`:
- vd
- vd2to3.vdx
- visidata
Consider using `uvx --from visidata <EXECUTABLE_NAME>` instead.
```

I've tried a few other things but none worked:

```
uvx --from git+https://github.com/saulpw/visidata apps/vdsql
uvx --from git+https://github.com/saulpw/visidata visidata/apps/vdsql
uvx --from git+https://github.com/saulpw/visidata/visidata/apps vdsql
uvx --from git+https://github.com/saulpw/visidata --directory visidata/apps/vdsql vdsql
uvx --from git+https://github.com/saulpw/visidata --project visidata/apps/vdsql vdsql
```

Did I miss something or is this just not (yet) supported?

---

_Comment by @charliermarsh on 2025-01-18 02:59_

I think the syntax you want is:

```
uvx --from git+https://github.com/saulpw/visidata#subdirectory=visidata/apps/vdsql vdsql
```

(This is part of PEP 508, not a uv-specific syntax.)

---

_Label `question` added by @charliermarsh on 2025-01-18 02:59_

---

_Closed by @charliermarsh on 2025-01-20 14:28_

---

_Comment by @lczyk on 2025-09-25 10:34_

~~@charliermarsh what if you also want to use the `repo@branch` syntax? both `git+https://github.com/user/repo#subdirectory=sub@branch` and `git+https://github.com/user/repo@branch#subdirectory=sub` don't seem to work (`uv 0.8.22` on linux)~~

EDIT: just found https://github.com/astral-sh/uv/issues/6093 which is exactly that question. worth linking from here i think

---
