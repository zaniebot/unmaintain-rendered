---
number: 17113
title: "`uv add -U package` claims unsatisfiable with pinned dependency and constraints-dependencies configured"
type: issue
state: closed
author: rogersei
labels:
  - bug
assignees: []
created_at: 2025-12-12T21:35:04Z
updated_at: 2025-12-13T03:03:51Z
url: https://github.com/astral-sh/uv/issues/17113
synced_at: 2026-01-07T13:12:19-06:00
---

# `uv add -U package` claims unsatisfiable with pinned dependency and constraints-dependencies configured

---

_Issue opened by @rogersei on 2025-12-12 21:35_

### Summary

This might be user error or a doc improvement, not a code bug.

Our pyproject.toml specifies pinned dependency versions (so package==x.y.z not ranges). It also defines package constraints in the constraints-dependencies, also a pinned version.

Specifically - starting with a working project with both dependencies and constraints-dependencies configured with `zapp==3.20.2`, change the constraints-dependencies to be `zapp==3.21.0`

My assumption is I could run `uv add -U zipp` and the dependency value and lock file would both get updated to the new constraint version - but I get this instead:

```
root@3c0e7cfdf989:/workspaces/neptune-bulk# uv add -U zipp
  × No solution found when resolving dependencies:
  ╰─▶ Because your project depends on zipp==3.20.2 and zipp==3.21.0, we can conclude that your project's
      requirements are unsatisfiable.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to
        skip locking and syncing.
```

using `uv add -P zipp` also failed.

Am I running the wrong command to trigger the package and pyproject.toml upgrade, or is there a technical issue here?

Changing the pinned project dependencies to be ranges did allow syncing to happen, but still doesn't update the target version

### Platform

ghcr.io/astral-sh/uv:python3.11-bookworm

### Version

0.9.15

### Python version

3.11

---

_Label `bug` added by @rogersei on 2025-12-12 21:35_

---

_Comment by @charliermarsh on 2025-12-13 03:03_

Unfortunately `uv add` doesn't upgrade existing bounds right now. I think it'd be best to track https://github.com/astral-sh/uv/issues/6794 -- we want to add a dedicated `uv upgrade` command or flow, it's on the near-term roadmap.

---

_Closed by @charliermarsh on 2025-12-13 03:03_

---
