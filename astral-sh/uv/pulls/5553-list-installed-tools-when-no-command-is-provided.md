```yaml
number: 5553
title: "List installed tools when no command is provided to `uv tool run`"
type: pull_request
state: merged
author: eth3lbert
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: tool-run-list-installed
created_at: 2024-07-29T08:16:58Z
updated_at: 2024-07-29T22:38:44Z
url: https://github.com/astral-sh/uv/pull/5553
synced_at: 2026-01-12T16:06:53Z
```

# List installed tools when no command is provided to `uv tool run`

---

_@eth3lbert_

## Summary

Part of #4024 

## Test Plan

Test cases included.


---

_Comment by @eth3lbert on 2024-07-29 09:12_

CI failures — seem unrelated to this change.

---

_Comment by @T-256 on 2024-07-29 10:31_

From Charlie:
> I think `uv tool run` should list the available executables, to start. That seems straightforward.

A tool can have multiple executables installed.

---

_Comment by @eth3lbert on 2024-07-29 10:58_

> From Charlie:
> 
> > I think `uv tool run` should list the available executables, to start. That seems straightforward.
> 
> A tool can have multiple executables installed.

Did I misunderstand the issue? I'm not sure I follow your point.

---

_Comment by @T-256 on 2024-07-29 20:15_

> Did I misunderstand the issue? I'm not sure I follow your point.

nvm, my bad. I didn't see the code and from title I thought it's going to list tool names only but not exposed entry points.

---

_@T-256 reviewed on 2024-07-29 20:20_

---

_Review comment by @T-256 on `crates/uv/tests/tool_run.rs`:809 on 2024-07-29 20:20_

Shouldn't it also show a message about what the below list means:
`Available tools and executables to run:`

---

_@eth3lbert reviewed on 2024-07-29 20:40_

---

_Review comment by @eth3lbert on `crates/uv/tests/tool_run.rs`:809 on 2024-07-29 20:40_

Thanks for the suggestion! However, since I'm simply reusing the `tool_list`, the output will exactly match `uv run list`. Additionally, if we want to display a message, we would probably expect the output of `uv run list` to change as well. I can make the change if desired, or perhaps it would be more suitable to make the change in a follow-up PR.

---

_@charliermarsh approved on 2024-07-29 22:03_

---

_Label `cli` added by @charliermarsh on 2024-07-29 22:03_

---

_Label `preview` added by @charliermarsh on 2024-07-29 22:03_

---

_Merged by @charliermarsh on 2024-07-29 22:12_

---

_Closed by @charliermarsh on 2024-07-29 22:12_

---

_Branch deleted on 2024-07-29 22:38_

---
