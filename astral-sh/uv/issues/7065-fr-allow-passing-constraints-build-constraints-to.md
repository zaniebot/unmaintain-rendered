---
number: 7065
title: "[FR] Allow passing --constraints/--build-constraints to uv build"
type: issue
state: closed
author: alex
labels:
  - enhancement
assignees: []
created_at: 2024-09-05T02:34:03Z
updated_at: 2024-09-05T18:46:38Z
url: https://github.com/astral-sh/uv/issues/7065
synced_at: 2026-01-10T01:24:10Z
---

# [FR] Allow passing --constraints/--build-constraints to uv build

---

_Issue opened by @alex on 2024-09-05 02:34_

`uv build` will download dependencies that are needed to build the sdist/wheel.

It'd be great if it were possible to pass `--constraints`/`--build-constraints` so we can enforce versions that are downloaded.

---

_Label `enhancement` added by @charliermarsh on 2024-09-05 02:34_

---

_Comment by @alex on 2024-09-05 12:50_

One note: `pip` currently will enforce a hash for the input file (e.g. `pip build --require-hashes -c constraints.txt my-sdist.tar.gz` will require a hash for `my-sdist.tar.gz`). I don't find this to be the expected behavior, I'd expect the constraints to cover the downloaded files, not the local input.

---

_Referenced in [astral-sh/uv#7082](../../astral-sh/uv/issues/7082.md) on 2024-09-05 13:27_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-05 13:29_

---

_Referenced in [astral-sh/uv#7085](../../astral-sh/uv/pulls/7085.md) on 2024-09-05 15:25_

---

_Closed by @charliermarsh on 2024-09-05 18:46_

---

_Closed by @charliermarsh on 2024-09-05 18:46_

---
