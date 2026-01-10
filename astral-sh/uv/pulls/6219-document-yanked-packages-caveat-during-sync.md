```yaml
number: 6219
title: Document yanked packages caveat during sync
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: zb/cli-yank-sync
created_at: 2024-08-19T17:43:52Z
updated_at: 2024-08-19T17:52:54Z
url: https://github.com/astral-sh/uv/pull/6219
synced_at: 2026-01-10T13:09:51Z
```

# Document yanked packages caveat during sync

---

_Pull request opened by @zanieb on 2024-08-19 17:43_

Closes https://github.com/astral-sh/uv/issues/5928

---

_Label `documentation` added by @zanieb on 2024-08-19 17:43_

---

_Label `preview` added by @zanieb on 2024-08-19 17:43_

---

_@ibraheemdev reviewed on 2024-08-19 17:49_

Should we should be more clear about what "installing from a lockfile" means, like "installing with a lockfile present"?

---

_Comment by @zanieb on 2024-08-19 17:49_

Don't we re-solve entirely if the lockfile isn't up to date now?

---

_Comment by @ibraheemdev on 2024-08-19 17:52_

Oh that's right. I guess we could always update this if we decide to rely on the lockfile for metadata.

---

_@ibraheemdev approved on 2024-08-19 17:52_

---

_Merged by @zanieb on 2024-08-19 17:52_

---

_Closed by @zanieb on 2024-08-19 17:52_

---

_Branch deleted on 2024-08-19 17:52_

---
