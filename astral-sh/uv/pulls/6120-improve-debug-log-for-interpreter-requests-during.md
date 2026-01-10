```yaml
number: 6120
title: Improve debug log for interpreter requests during project commands
type: pull_request
state: merged
author: zanieb
labels:
  - tracing
  - preview
assignees: []
merged: true
base: main
head: zb/interpreter-request
created_at: 2024-08-15T16:49:28Z
updated_at: 2024-08-16T01:31:00Z
url: https://github.com/astral-sh/uv/pull/6120
synced_at: 2026-01-10T13:09:50Z
```

# Improve debug log for interpreter requests during project commands

---

_Pull request opened by @zanieb on 2024-08-15 16:49_

While it's slightly more convenient to log this where we were, it was pretty unhelpful e.g.

```
DEBUG Interpreter meets the requested Python: `Python >=3.9`
```

What interpreter are we referring to here?

---

_Label `tracing` added by @zanieb on 2024-08-15 16:49_

---

_Label `preview` added by @zanieb on 2024-08-15 16:49_

---

_@charliermarsh reviewed on 2024-08-15 16:53_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:151 on 2024-08-15 16:53_

Honestly should we just get rid of this method? It's `requested_python.map_or(true, |request| request.satisfied(interpreter, cache))`.

---

_@charliermarsh reviewed on 2024-08-15 16:53_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:225 on 2024-08-15 16:53_

Is this used anywhere else?

---

_@charliermarsh approved on 2024-08-15 16:53_

---

_@zanieb reviewed on 2024-08-15 16:55_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:151 on 2024-08-15 16:55_

I think it used to be used in more places? You're probably right though.

---

_@charliermarsh reviewed on 2024-08-15 17:02_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:151 on 2024-08-15 17:02_

Yeah looks like this is the only one left. IMO we should just inline it.

---

_Merged by @zanieb on 2024-08-16 01:31_

---

_Closed by @zanieb on 2024-08-16 01:31_

---

_Branch deleted on 2024-08-16 01:31_

---
