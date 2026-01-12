```yaml
number: 8190
title: Load Git trees lazily on checkout
type: pull_request
state: open
author: zanieb
labels:
  - performance
assignees: []
draft: true
base: main
head: zb/partial-clone-2
created_at: 2024-10-14T21:55:48Z
updated_at: 2025-04-14T08:00:32Z
url: https://github.com/astral-sh/uv/pull/8190
synced_at: 2026-01-12T16:08:12Z
```

# Load Git trees lazily on checkout

---

_@zanieb_

Exploring https://github.com/astral-sh/uv/pull/3950

---

_Label `performance` added by @zanieb on 2024-10-14 21:55_

---

_Comment by @zanieb on 2024-10-14 22:49_

Turns out this is still really hard. Some weird behavior due to our local clone, I think?

---

_Comment by @zanieb on 2024-10-14 23:19_

I suspect that we are doing a dynamic fetch and the promisor is the sparse local tree?

> Since almost all Git code currently expects any referenced object to be present locally and because we do not want to force every command to do a dry-run first, a fallback mechanism is added to allow Git to attempt to dynamically fetch missing objects from promisor remotes.
> 
> When the normal object lookup fails to find an object, Git invokes promisor_remote_get_direct() to try to get the object from a promisor remote and then retry the object lookup. This allows objects to be "faulted in" without complicated prediction algorithms.
>
> Dynamic object fetching will only ask promisor remotes for missing objects. We assume that promisor remotes have a complete view of the repository and can satisfy all such requests.

Which fails since https://github.com/git/git/commit/301f1e3ac1531dc3a15064a06b24fa98f02a3b78

---
