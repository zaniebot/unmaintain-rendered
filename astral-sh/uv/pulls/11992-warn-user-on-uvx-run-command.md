```yaml
number: 11992
title: "Warn user on `uvx run` command"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/w
created_at: 2025-03-05T22:04:30Z
updated_at: 2025-03-06T02:15:20Z
url: https://github.com/astral-sh/uv/pull/11992
synced_at: 2026-01-12T16:10:05Z
```

# Warn user on `uvx run` command

---

_@charliermarsh_

## Summary

If a user invokes `uvx run ...`, we hint them towards `uvx`. Otherwise, this invokes the `run` package, which is unmaintained on PyPI.

If the user is _only_ using PyPI, we show an interactive prompt here; otherwise, we just show a dedicated warning on error.

Closes https://github.com/astral-sh/uv/issues/11982.

## Test Plan

Prompting to success:

![Screenshot 2025-03-05 at 5 00 47 PM](https://github.com/user-attachments/assets/d8180606-94e1-41df-b799-19b8ba57e662)

If you use `--from`, we avoid the prompt and hint:

![Screenshot 2025-03-05 at 5 03 26 PM](https://github.com/user-attachments/assets/59919390-01d3-4ddf-97bc-bb857ae9f8b0)

If you provide another index, we don't prompt, but we do warn on failure:

![Screenshot 2025-03-05 at 5 03 43 PM](https://github.com/user-attachments/assets/0cc72c36-5744-48f1-aeff-4a214190d6fd)


---

_Review requested from @zanieb by @charliermarsh on 2025-03-05 22:04_

---

_Label `error messages` added by @charliermarsh on 2025-03-05 22:04_

---

_@zanieb reviewed on 2025-03-05 22:12_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:161 on 2025-03-05 22:12_

```suggestion
    // the `run` package is guaranteed to come from PyPI.
```

Clarifies the motivation a little, could refer to `ruff` otherwise

---

_@zanieb reviewed on 2025-03-05 22:12_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:161 on 2025-03-05 22:12_

Might want to note `run` is reserved on PyPI?

---

_@zanieb approved on 2025-03-05 22:12_

---

_Merged by @charliermarsh on 2025-03-06 02:15_

---

_Closed by @charliermarsh on 2025-03-06 02:15_

---

_Branch deleted on 2025-03-06 02:15_

---
