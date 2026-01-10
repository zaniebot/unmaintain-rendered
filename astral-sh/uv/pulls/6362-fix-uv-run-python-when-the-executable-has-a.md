```yaml
number: 6362
title: "Fix `uv run python` when the executable has a different name"
type: pull_request
state: closed
author: zanieb
labels:
  - bug
assignees: []
draft: true
base: zb/run-python-pin
head: zb/run-python-missing
created_at: 2024-08-21T17:57:29Z
updated_at: 2024-08-21T18:22:21Z
url: https://github.com/astral-sh/uv/pull/6362
synced_at: 2026-01-10T13:09:51Z
```

# Fix `uv run python` when the executable has a different name

---

_Pull request opened by @zanieb on 2024-08-21 17:57_

_No description provided._

---

_Label `bug` added by @zanieb on 2024-08-21 17:57_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:757 on 2024-08-21 17:57_

What's the idiomatic way to write this..?

---

_@zanieb reviewed on 2024-08-21 17:57_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:745 on 2024-08-21 18:00_

Could this not resolve to some Python that doesn't match the intended interpreter? Like you run `uv run -p 3.9`, but we then pick up Python 3.12 that's on your path?

---

_@charliermarsh reviewed on 2024-08-21 18:00_

---

_@zanieb reviewed on 2024-08-21 18:01_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:745 on 2024-08-21 18:01_

Yeah but what if you have a `python` executable in the interpreter bin that does some special behavior? Should we unconditionally switch to the sys executable? Should we just look in the interpreter bin instead of the whole PATH here?

---

_@zanieb reviewed on 2024-08-21 18:02_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:745 on 2024-08-21 18:02_

Seems problematic both ways. I'm okay with unconditional replacement, I guess?

---

_@zanieb reviewed on 2024-08-21 18:09_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:745 on 2024-08-21 18:09_

Here's that approach https://github.com/astral-sh/uv/pull/6363

---

_Closed by @zanieb on 2024-08-21 18:22_

---
