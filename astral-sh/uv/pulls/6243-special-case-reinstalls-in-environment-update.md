```yaml
number: 6243
title: Special-case reinstalls in environment update summaries
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: zb/reinstall
created_at: 2024-08-20T03:57:10Z
updated_at: 2024-08-20T15:35:34Z
url: https://github.com/astral-sh/uv/pull/6243
synced_at: 2026-01-12T16:07:17Z
```

# Special-case reinstalls in environment update summaries

---

_@zanieb_

e.g.

```
❯ uv pip install httpx==0.26.0 --reinstall-package httpx
Resolved 7 packages in 67ms
Prepared 1 package in 0.95ms
Uninstalled 1 package in 2ms
Installed 1 package in 12ms
 ~ httpx==0.26.0
```

```
❯ uv add httpx==0.26.0
warning: `uv add` is experimental and may change without warning
warning: `uv.sources` is experimental and may change without warning
Resolved 15 packages in 23ms
   Built example @ file:///Users/zb/workspace/example
Prepared 2 packages in 187ms
Uninstalled 2 packages in 2ms
Installed 2 packages in 2ms
 ~ example==0.1.0 (from file:///Users/zb/workspace/example)
 - httpx==0.27.0
 + httpx==0.26.0
```

Motivated by trying to reduce the diff for project updates in `uv add`. I think it makes sense in general though. We'll also want a special output for upgrades in the future, e.g. reducing the `httpx` changes in the second example to a single line indicating the original and subsequent distribution versions.

I'd like to have

> Reinstalled 1 package in 2ms

but it seems non-trivial to show the timing for it? I think we'd need to determine what we want to call a "Reinstall" during the operations and split doing them from the rest of the uninstalls and installs.

---

_Label `enhancement` added by @zanieb on 2024-08-20 03:57_

---

_Label `cli` added by @zanieb on 2024-08-20 03:57_

---

_Marked ready for review by @zanieb on 2024-08-20 14:30_

---

_Review requested from @charliermarsh by @zanieb on 2024-08-20 14:59_

---

_Comment by @zanieb on 2024-08-20 15:00_

It looks like we could determine reinstalls and upgrades from the `Plan`. I've got some follow-up work-in-progress that shifts to doing that, but I don't think it blocks this change.

---

_@charliermarsh approved on 2024-08-20 15:08_

This looks reasonable to me.

---

_Merged by @zanieb on 2024-08-20 15:35_

---

_Closed by @zanieb on 2024-08-20 15:35_

---

_Branch deleted on 2024-08-20 15:35_

---
