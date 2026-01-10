```yaml
number: 14569
title: Detect cases where uv partially removed a virtual environment and retry
type: pull_request
state: closed
author: zanieb
labels:
  - bug
assignees: []
draft: true
base: main
head: zb/venv-rm-marker
created_at: 2025-07-11T18:25:59Z
updated_at: 2025-07-22T11:43:11Z
url: https://github.com/astral-sh/uv/pull/14569
synced_at: 2026-01-10T06:53:02Z
```

# Detect cases where uv partially removed a virtual environment and retry

---

_Pull request opened by @zanieb on 2025-07-11 18:25_

This is a sort of janky fix for https://github.com/astral-sh/uv/issues/13986 where we use a `.venv/.uv-partial-rm` marker to indicate that we attempted to delete an environment but failed then use that to allow deletion of an environment despite a missing `pyvenv.cfg` marker.

I think we may better be served by another approach, but this was an idea I wanted to sketch.

---

_Label `bug` added by @zanieb on 2025-07-11 18:26_

---

_Closed by @zanieb on 2025-07-22 11:43_

---
