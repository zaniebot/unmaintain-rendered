```yaml
number: 273
title: Clean up some path handling in the schemastore script
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/schema-store
created_at: 2025-05-08T14:01:45Z
updated_at: 2025-05-08T14:32:20Z
url: https://github.com/astral-sh/ty/pull/273
synced_at: 2026-01-10T02:34:10Z
```

# Clean up some path handling in the schemastore script

---

_Pull request opened by @zanieb on 2025-05-08 14:01_

Mostly making sure that the script is robust to alternative working directories, and some stylistic nits.

Following up on https://github.com/astral-sh/ty/pull/65#discussion_r2077839514 — not to be annoying, but using `git` to find the root _can_ be wrong here


```
❯ cd ruff
❯ uv run --only-dev ../scripts/update_schemastore.py
> /Users/zb/workspace/ty/scripts/update_schemastore.py(146)main()
-> breakpoint()
(Pdb) print(root)
/Users/zb/workspace/ty/ruff
```

---

_Label `internal` added by @zanieb on 2025-05-08 14:32_

---

_Merged by @zanieb on 2025-05-08 14:32_

---

_Closed by @zanieb on 2025-05-08 14:32_

---

_Branch deleted on 2025-05-08 14:32_

---
