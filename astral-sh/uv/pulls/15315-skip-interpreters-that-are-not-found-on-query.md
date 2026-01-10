```yaml
number: 15315
title: Skip interpreters that are not found on query
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/not-found-on-query
created_at: 2025-08-15T20:28:19Z
updated_at: 2025-08-18T15:42:57Z
url: https://github.com/astral-sh/uv/pull/15315
synced_at: 2026-01-10T06:44:33Z
```

# Skip interpreters that are not found on query

---

_Pull request opened by @zanieb on 2025-08-15 20:28_

Closes https://github.com/astral-sh/uv/issues/12155

We already throw this error earlier if we cannot find the interpreter https://github.com/astral-sh/uv/blob/c318e8860e69077fdfe8fff7356985e684591b40/crates/uv-python/src/interpreter.rs#L1039

Why the pyenv-win shim _exists_ but fails to run with a not found error is beyond me. I think I'll take the incremental improvement here by just ignoring it. We can try to support their shims later?

#15317 confirms the fix.

---

_Label `bug` added by @zanieb on 2025-08-15 20:28_

---

_Marked ready for review by @zanieb on 2025-08-15 22:54_

---

_@geofft approved on 2025-08-18 15:14_

Just so I understand, this is the Windows equivalent of `execve` returning `ENOENT`?

---

_Comment by @zanieb on 2025-08-18 15:42_

I'm not entirely sure, but yes? This change is not Windows specific though.

---

_Merged by @zanieb on 2025-08-18 15:42_

---

_Closed by @zanieb on 2025-08-18 15:42_

---

_Branch deleted on 2025-08-18 15:42_

---
