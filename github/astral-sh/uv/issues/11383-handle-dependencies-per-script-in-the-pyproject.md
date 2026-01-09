---
number: 11383
title: "Handle dependencies per script, in the `pyproject.toml`"
type: issue
state: open
author: Arno500
labels:
  - enhancement
assignees: []
created_at: 2025-02-10T13:09:44Z
updated_at: 2025-02-10T13:09:44Z
url: https://github.com/astral-sh/uv/issues/11383
synced_at: 2026-01-07T13:12:18-06:00
---

# Handle dependencies per script, in the `pyproject.toml`

---

_Issue opened by @Arno500 on 2025-02-10 13:09_

### Summary

There is a really interesting feature that allows a different set of dependencies per script: https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies
Unfortunately, in project mode, if a script requires a different set of dependency than another one, there is no way to specify those in the `pyproject.toml`.

An elegant way would be to have a optional group, and specify somewhere in the config that a specific script requires a specific optional group.

### Example

https://github.com/criteo/hwbench
Currently here the way is to install manually the optional `graph` group.
Being able to do `uv run hwgraph` immediately after cloning the repo (and automagically installing the deps) would be great for the UX

---

_Label `enhancement` added by @Arno500 on 2025-02-10 13:09_

---
