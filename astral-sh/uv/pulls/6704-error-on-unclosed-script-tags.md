```yaml
number: 6704
title: Error on unclosed script tags
type: pull_request
state: merged
author: charliermarsh
labels:
  - tracing
assignees: []
merged: true
base: main
head: charlie/w
created_at: 2024-08-27T17:19:07Z
updated_at: 2024-08-27T17:47:12Z
url: https://github.com/astral-sh/uv/pull/6704
synced_at: 2026-01-12T16:07:29Z
```

# Error on unclosed script tags

---

_@charliermarsh_

Should this be user-facing by default? It seems annoying because then it's unavoidable if you (for whatever reason) have an intentionally unclosed tag.

Motivated by https://github.com/astral-sh/uv/issues/6700.

---

_Label `tracing` added by @charliermarsh on 2024-08-27 17:19_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-27 17:19_

---

_Comment by @zanieb on 2024-08-27 17:20_

I'd say user-facing unless someone complains and shows us a compelling use-case for an unclosed tag.

---

_@zanieb approved on 2024-08-27 17:20_

Test case please <3

---

_Comment by @charliermarsh on 2024-08-27 17:24_

Making it user-facing means it's easier to test :)

---

_Review requested from @zanieb by @charliermarsh on 2024-08-27 17:34_

---

_@zanieb approved on 2024-08-27 17:37_

Ah user-facing like an error instead of `warn_user`. Interesting. I'm down though.

---

_Comment by @charliermarsh on 2024-08-27 17:39_

Let's give it a try...

---

_Renamed from "Warn on unclosed script tags" to "Error on unclosed script tags" by @zanieb on 2024-08-27 17:44_

---

_Merged by @charliermarsh on 2024-08-27 17:47_

---

_Closed by @charliermarsh on 2024-08-27 17:47_

---

_Branch deleted on 2024-08-27 17:47_

---
