```yaml
number: 16857
title: "Avoid eagerly reading input streams in `-r`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/empty
created_at: 2025-11-26T03:29:34Z
updated_at: 2025-11-26T03:55:10Z
url: https://github.com/astral-sh/uv/pull/16857
synced_at: 2026-01-10T05:58:11Z
```

# Avoid eagerly reading input streams in `-r`

---

_Pull request opened by @charliermarsh on 2025-11-26 03:29_

## Summary

I think the comment should explain it.

Closes https://github.com/astral-sh/uv/issues/16856.


---

_Label `bug` added by @charliermarsh on 2025-11-26 03:29_

---

_Marked ready for review by @charliermarsh on 2025-11-26 03:30_

---

_@zanieb reviewed on 2025-11-26 03:47_

---

_Review comment by @zanieb on `crates/uv-requirements/src/sources.rs`:74 on 2025-11-26 03:47_

What do you mean by "only `requirements.txt` format is supported anyway"? It seems like you could do this with another file? 

---

_Comment by @zanieb on 2025-11-26 03:48_

Not blocking the bug fix, but... why not just hold on to the contents in memory if we need to sniff it? It seems a little weird to constrain streamed contents to `requirements.txt`?

---

_Comment by @zanieb on 2025-11-26 03:49_

How does this relate to https://github.com/astral-sh/uv/pull/16855 ?

---

_Comment by @zanieb on 2025-11-26 03:49_

I presume this regressed in https://github.com/astral-sh/uv/pull/16805 / #16744 ?

---

_@zanieb approved on 2025-11-26 03:51_

---

_Comment by @charliermarsh on 2025-11-26 03:55_

Yes, it regressed there; #16855 fixed one manifestation of this issue, but the problem is more general; yes, we should be storing the contents in-memory but this brings us back into parity with the code prior to those changes and fixes the regression.

---

_Merged by @charliermarsh on 2025-11-26 03:55_

---

_Closed by @charliermarsh on 2025-11-26 03:55_

---

_Branch deleted on 2025-11-26 03:55_

---
