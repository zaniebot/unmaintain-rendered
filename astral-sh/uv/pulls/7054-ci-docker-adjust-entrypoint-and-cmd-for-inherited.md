```yaml
number: 7054
title: "ci(docker)!: adjust entrypoint and cmd for inherited images"
type: pull_request
state: merged
author: samypr100
labels:
  - releases
assignees: []
merged: true
base: main
head: additional-tags-entrypoint-adjustment
created_at: 2024-09-04T23:00:49Z
updated_at: 2024-09-04T23:59:24Z
url: https://github.com/astral-sh/uv/pull/7054
synced_at: 2026-01-12T16:07:39Z
```

# ci(docker)!: adjust entrypoint and cmd for inherited images

---

_@samypr100_

## Summary

Closes https://github.com/astral-sh/uv/issues/7030

This removes our custom `ENTRYPOINT` just for the additional docker tags, and makes it empty (to avoid possible upstream surprises if any) and moves running uv to `CMD` for consistency.

This approach is probably the in-between solution from the discussion in https://github.com/astral-sh/uv/issues/7030#issuecomment-2329443719 and would work for everyone's use cases.

## Test Plan

Tested release workflow in https://github.com/samypr100/uv/actions/runs/10711049920

The default CMD still gives a nice default.

```shell
> docker run ghcr.io/samypr100/uv:0.4.5-alpine
An extremely fast Python package manager.

Usage: uv [OPTIONS] <COMMAND>

Commands:
  run      Run a command or script
  init     Create a new project
  add      Add dependencies to the project
  remove   Remove dependencies from the project
  sync     Update the project's environment
  lock     Update the project's lockfile
  export   Export the project's lockfile to an alternate format
  tree     Display the project's dependency tree
  tool     Run and install commands provided by Python packages
  python   Manage Python versions and installations
  pip      Manage Python packages with a pip-compatible interface
  venv     Create a virtual environment
  build    Build Python packages into source distributions and wheels
  cache    Manage uv's cache
  version  Display uv's version
  help     Display documentation for a command
```


---

_@zanieb approved on 2024-09-04 23:39_

Yeah makes sense to me

---

_Comment by @zanieb on 2024-09-04 23:40_

This is technically breaking but since the images are so new I'm tempted to go with it.

---

_Marked ready for review by @samypr100 on 2024-09-04 23:42_

---

_Comment by @samypr100 on 2024-09-04 23:43_

> This is technically breaking but since the images are so new I'm tempted to go with it.

Yea, I agree.

---

_Label `release` added by @zanieb on 2024-09-04 23:55_

---

_Merged by @zanieb on 2024-09-04 23:55_

---

_Closed by @zanieb on 2024-09-04 23:55_

---

_Branch deleted on 2024-09-04 23:59_

---
