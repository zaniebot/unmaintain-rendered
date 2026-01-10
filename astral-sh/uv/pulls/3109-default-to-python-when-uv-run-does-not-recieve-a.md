```yaml
number: 3109
title: "Default to `python` when `uv run` does not recieve a command"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - cli
assignees: []
merged: true
base: main
head: zb/uv-run-python
created_at: 2024-04-17T22:14:59Z
updated_at: 2024-04-19T15:15:39Z
url: https://github.com/astral-sh/uv/pull/3109
synced_at: 2026-01-10T14:43:31Z
```

# Default to `python` when `uv run` does not recieve a command

---

_Pull request opened by @zanieb on 2024-04-17 22:14_

This means that a bare `uv run` invocation drops you into a REPL.

This behavior is internally controversial, and may best be served by a dedicated `uv repl` command. I would imagine it's important to fail if no command is given in _some_ circumstances, but those may be resolved by _not_ doing this if we do not detect a TTY.

Regardless, I'm interested in giving this a try for a bit during this experimental phase.

---

_Label `internal` added by @zanieb on 2024-04-17 22:15_

---

_Label `cli` added by @zanieb on 2024-04-17 22:15_

---

_@zanieb reviewed on 2024-04-17 22:49_

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:50 on 2024-04-17 22:49_

I guess we could use the direct path to the Python in the virtual environment but that seems overkill to me?

---

_Comment by @konstin on 2024-04-18 08:55_

The alternative is adding a `tool.uv.default-run` and use this instead of repl, with the `python` shim from rye creating a repl instead.

---

_@konstin approved on 2024-04-18 08:55_

---

_Marked ready for review by @zanieb on 2024-04-19 15:06_

---

_Merged by @zanieb on 2024-04-19 15:15_

---

_Closed by @zanieb on 2024-04-19 15:15_

---

_Branch deleted on 2024-04-19 15:15_

---
