```yaml
number: 5983
title: "Improve top-level help for `uv tool` commands"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - cli
  - preview
assignees: []
merged: true
base: main
head: zb/tool-cli-top-level
created_at: 2024-08-09T22:57:19Z
updated_at: 2024-08-12T17:35:51Z
url: https://github.com/astral-sh/uv/pull/5983
synced_at: 2026-01-10T13:31:54Z
```

# Improve top-level help for `uv tool` commands

---

_Pull request opened by @zanieb on 2024-08-09 22:57_

More work needs to be done for all of the options


---

_Label `documentation` added by @zanieb on 2024-08-09 22:57_

---

_Label `cli` added by @zanieb on 2024-08-09 22:57_

---

_Label `preview` added by @zanieb on 2024-08-09 22:57_

---

_Review requested from @charliermarsh by @zanieb on 2024-08-09 23:59_

---

_@charliermarsh reviewed on 2024-08-10 04:01_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2622 on 2024-08-10 04:01_

I might say... uvx is provided as a convenient alias for uv tool run.

---

_@charliermarsh reviewed on 2024-08-10 04:02_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2629 on 2024-08-10 04:02_

"when invoking tools, as package installation..."

(Is this piece necessary?)

---

_@charliermarsh approved on 2024-08-10 04:02_

---

_@zanieb reviewed on 2024-08-10 14:04_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2629 on 2024-08-10 14:04_

I'm a little hesitant on it, but I want to warn about dependency confusion attacks e.g. `uvx foobar` when `foobar` is actually provided by an official `foobar-cli` is a real risk.

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2622 on 2024-08-10 14:05_

```suggestion
    /// `uvx` is provided as a convenient alias for `uv tool run`, their behavior is identical.
```

---

_@zanieb reviewed on 2024-08-10 14:05_

---

_Merged by @zanieb on 2024-08-12 17:35_

---

_Closed by @zanieb on 2024-08-12 17:35_

---

_Branch deleted on 2024-08-12 17:35_

---
