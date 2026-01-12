```yaml
number: 10521
title: "Fix `OSTYPE` unset error in virtualenv activate script"
type: pull_request
state: open
author: zanieb
labels: []
assignees: []
draft: true
base: zb/fix-cygpath
head: zb/fix-ostype
created_at: 2025-01-11T19:35:08Z
updated_at: 2025-04-22T18:40:54Z
url: https://github.com/astral-sh/uv/pull/10521
synced_at: 2026-01-12T16:09:20Z
```

# Fix `OSTYPE` unset error in virtualenv activate script

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/10498

---

_Review requested from @Gankra by @Gankra on 2025-01-13 14:25_

---

_Comment by @zanieb on 2025-04-21 19:37_

@Gankra is this and https://github.com/astral-sh/uv/pull/10492 relevant anymore?

---

_Comment by @Gankra on 2025-04-22 18:40_

Yes these are still fixing the issue of the script not working in that one shell that does extra validation.

---
