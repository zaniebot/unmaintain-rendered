---
number: 7228
title: Add notes about dependency normalisation in the documentation
type: issue
state: open
author: mkniewallner
labels: []
assignees: []
created_at: 2024-09-09T19:02:44Z
updated_at: 2024-09-09T19:02:44Z
url: https://github.com/astral-sh/uv/issues/7228
synced_at: 2026-01-07T13:12:17-06:00
---

# Add notes about dependency normalisation in the documentation

---

_Issue opened by @mkniewallner on 2024-09-09 19:02_

[Dependency normalisation](https://packaging.python.org/en/latest/specifications/name-normalization/) is applied at multiple levels in uv. This might be something that is not obvious to people that do not know too much about Python packaging internals, or are new to Python.

Most of the times it's not something that people need to worry about too much, but maybe adding small notes about that in some parts of the documentation where it would be relevant could makes sense (adding a dedicated section in a page + linking to it for example)? I'm mostly thinking of cases where it's even less obvious that uv does that, for instance when mapping a dependency name to a source (https://docs.astral.sh/uv/concepts/dependencies/#dependency-sources). For context and as an example that this could have been useful, at least for sources, I recently had to test uv's behaviour in https://github.com/renovatebot/renovate/pull/31270#discussion_r1750746936.

---
