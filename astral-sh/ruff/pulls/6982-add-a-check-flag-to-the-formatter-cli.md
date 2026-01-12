```yaml
number: 6982
title: Add a --check flag to the formatter CLI
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/format-check
created_at: 2023-08-29T16:26:37Z
updated_at: 2023-08-29T16:46:56Z
url: https://github.com/astral-sh/ruff/pull/6982
synced_at: 2026-01-12T02:45:38Z
```

# Add a --check flag to the formatter CLI

---

_Pull request opened by @charliermarsh on 2023-08-29 16:26_

## Summary

Returns an exit code of 1 if any files would be reformatted:

```
ruff on  charlie/format-check:main [$?⇡] is 📦 v0.0.286 via 🐍 v3.11.2 via 🦀 v1.72.0
❯ cargo run -p ruff_cli -- format foo.py --check
   Compiling ruff_cli v0.0.286 (/Users/crmarsh/workspace/ruff/crates/ruff_cli)
    Finished dev [unoptimized + debuginfo] target(s) in 1.69s
     Running `target/debug/ruff format foo.py --check`
warning: `ruff format` is a work-in-progress, subject to change at any time, and intended only for experimentation.
1 file would be reformatted
ruff on  charlie/format-check:main [$?⇡] is 📦 v0.0.286 via 🐍 v3.11.2 via 🦀 v1.72.0 took 2s
❯ echo $?
1
```

Closes #6966.


---

_Label `formatter` added by @charliermarsh on 2023-08-29 16:27_

---

_@zanieb approved on 2023-08-29 16:28_

nice

---

_@zanieb reviewed on 2023-08-29 16:29_

---

_Review comment by @zanieb on `crates/ruff_cli/src/commands/format.rs`:28 on 2023-08-29 16:29_

I was wondering why some enums had `is` methods!

---

_@MichaReiser approved on 2023-08-29 16:38_

---

_Merged by @charliermarsh on 2023-08-29 16:40_

---

_Closed by @charliermarsh on 2023-08-29 16:40_

---

_Branch deleted on 2023-08-29 16:40_

---

_Comment by @github-actions[bot] on 2023-08-29 16:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
