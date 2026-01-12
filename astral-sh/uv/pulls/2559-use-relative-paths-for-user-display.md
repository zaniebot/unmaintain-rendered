```yaml
number: 2559
title: Use relative paths for user display
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/relativize
created_at: 2024-03-20T01:15:35Z
updated_at: 2024-03-20T13:52:51Z
url: https://github.com/astral-sh/uv/pull/2559
synced_at: 2026-01-12T16:05:06Z
```

# Use relative paths for user display

---

_@charliermarsh_

## Summary

This PR changes our user-facing representation for paths to use relative paths, when the path is within the current working directory. This mirrors what we do in Ruff. (If the path is _outside_ the current working directory, we print an absolute path.)

Before:

```shell
❯ uv venv .venv2
Using Python 3.12.2 interpreter at: /Users/crmarsh/workspace/uv/.venv/bin/python3
Creating virtualenv at: .venv2
Activate with: source .venv2/bin/activate
```

After:

```shell
❯ cargo run venv .venv2
    Finished dev [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/uv venv .venv2`
Using Python 3.12.2 interpreter at: .venv/bin/python3
Creating virtualenv at: .venv2
Activate with: source .venv2/bin/activate
```

Note that we still want to use the existing `.simplified_display()` anywhere that the path is being simplified, but _still_ intended for machine consumption (e.g., when passing to `.current_dir()`).


---

_Review requested from @zanieb by @charliermarsh on 2024-03-20 01:15_

---

_Comment by @zanieb on 2024-03-20 01:28_

+1 I prefer this strongly.

I would be careful with cases where the user provides something relative but stepping upwards like `./../../foo`

---

_Label `cli` added by @charliermarsh on 2024-03-20 02:40_

---

_Marked ready for review by @charliermarsh on 2024-03-20 02:45_

---

_Converted to draft by @charliermarsh on 2024-03-20 02:58_

---

_Marked ready for review by @charliermarsh on 2024-03-20 04:17_

---

_Review request for @zanieb removed by @charliermarsh on 2024-03-20 04:17_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-20 04:17_

---

_Review requested from @konstin by @charliermarsh on 2024-03-20 04:17_

---

_Review comment by @konstin on `crates/uv/tests/venv.rs`:33 on 2024-03-20 09:46_

Do we still need this filter?

---

_@konstin approved on 2024-03-20 09:47_

Much better

---

_@charliermarsh reviewed on 2024-03-20 13:52_

---

_Review comment by @charliermarsh on `crates/uv/tests/venv.rs`:33 on 2024-03-20 13:52_

Yeah, for some reason on macOS these paths are sometimes prefixed with `/private/`

---

_Merged by @charliermarsh on 2024-03-20 13:52_

---

_Closed by @charliermarsh on 2024-03-20 13:52_

---

_Branch deleted on 2024-03-20 13:52_

---
