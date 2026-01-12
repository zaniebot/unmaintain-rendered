```yaml
number: 3110
title: "Allow `uv run` to execute Python scripts directly"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - cli
assignees: []
merged: true
base: main
head: zb/uv-run-script
created_at: 2024-04-17T22:40:58Z
updated_at: 2024-04-19T21:29:58Z
url: https://github.com/astral-sh/uv/pull/3110
synced_at: 2026-01-12T16:05:26Z
```

# Allow `uv run` to execute Python scripts directly

---

_@zanieb_

e.g. `uv run foo.py` implies `python foo.py`

Future work includes #3096 


---

_Label `internal` added by @zanieb on 2024-04-17 22:40_

---

_Label `cli` added by @zanieb on 2024-04-17 22:40_

---

_@zanieb reviewed on 2024-04-17 22:41_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1636 on 2024-04-17 22:41_

We need this to insert a properly insert a path into the arguments, but this seems generally "more correct".

We'll want to do this for `target` too.


---

_@zanieb reviewed on 2024-04-17 22:47_

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:58 on 2024-04-17 22:47_

I guess this is vaguely not performant but not meaningful?

---

_Marked ready for review by @zanieb on 2024-04-19 15:05_

---

_Merged by @zanieb on 2024-04-19 21:29_

---

_Closed by @zanieb on 2024-04-19 21:29_

---

_Branch deleted on 2024-04-19 21:29_

---
