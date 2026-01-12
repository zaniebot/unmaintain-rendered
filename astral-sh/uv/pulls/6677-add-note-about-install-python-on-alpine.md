```yaml
number: 6677
title: Add note about install python on alpine
type: pull_request
state: merged
author: konstin
labels:
  - documentation
assignees: []
merged: true
base: main
head: konsti/alpine-note
created_at: 2024-08-27T08:00:10Z
updated_at: 2024-08-27T16:03:57Z
url: https://github.com/astral-sh/uv/pull/6677
synced_at: 2026-01-12T16:07:28Z
```

# Add note about install python on alpine

---

_@konstin_

When not using a python base image and using alpine, you need to install python by yourself. You should also pin the python version when doing so; currently, i see only python 3.12 in the alpine repository.


---

_Label `documentation` added by @konstin on 2024-08-27 08:00_

---

_Review requested from @zanieb by @konstin on 2024-08-27 08:02_

---

_@zanieb reviewed on 2024-08-27 11:18_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:81 on 2024-08-27 11:18_

```suggestion
    uv does not yet support installing Python on musl-based distributions. For example, if you are using an
```

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:82 on 2024-08-27 11:18_

```suggestion
    Alpine Linux base image that doesn't have Python installed, you need to add it with
    the system package manager:
```

---

_@zanieb reviewed on 2024-08-27 11:18_

---

_@zanieb reviewed on 2024-08-27 14:32_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:79 on 2024-08-27 14:32_

Sorry I should have noticed this in the first run. This is kind of a weird spot? Can we put this in a new section at the bottom or something? It feels quite prominent here and we're not talking about installing Python at all.

---

_Comment by @zanieb on 2024-08-27 14:33_

It looks like my comments were marked as addressed but there's no change?

---

_Comment by @konstin on 2024-08-27 14:53_

I had accepted both of them, not sure what the merge had done here.

---

_@zanieb approved on 2024-08-27 16:03_

Thanks!

---

_Merged by @zanieb on 2024-08-27 16:03_

---

_Closed by @zanieb on 2024-08-27 16:03_

---

_Branch deleted on 2024-08-27 16:03_

---
