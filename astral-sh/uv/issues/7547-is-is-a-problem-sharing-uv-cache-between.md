```yaml
number: 7547
title: Is is a problem sharing uv cache between different versions
type: issue
state: closed
author: freand76
labels:
  - question
  - cache
assignees: []
created_at: 2024-09-19T13:00:30Z
updated_at: 2024-09-25T22:13:04Z
url: https://github.com/astral-sh/uv/issues/7547
synced_at: 2026-01-10T04:45:10Z
```

# Is is a problem sharing uv cache between different versions

---

_Issue opened by @freand76 on 2024-09-19 13:00_

We use uv in a CI/CD environment where the .cache is shared within a build node.

When building from the tip of the repository the uv version will always be increasing 0.1.x -> 0.2.x - > ... > 0.4.x
But when running older change set och when recreating an old build there might be a jump for version 0.4.x to version 0.2.x (version decreasing).

Do you consider it a problem sharing the cache between uv versions?
Is it necessary that the version is **static** or **strictly increasing** when using the cache?

---

_Comment by @zanieb on 2024-09-19 13:26_

Yeah it is important that the version is strictly increasing. While we generally try to avoid this, there are cases where old uv versions will not be able to read the cache from a new version. The version should not need to be static though.

---

_Label `question` added by @zanieb on 2024-09-19 13:26_

---

_Label `cache` added by @zanieb on 2024-09-19 13:26_

---

_Comment by @zanieb on 2024-09-19 13:27_

(We should probably document this cc @charliermarsh)

---

_Comment by @charliermarsh on 2024-09-19 13:35_

It should _typically_ be ok because we version the cache buckets, so if we change the schema, we'll use a different cache version. But there are occasional bugs (like we saw in 0.4.10) that would make that not the case. I would say, in general, it should work -- modulo bugs.

---

_Comment by @freand76 on 2024-09-19 13:37_

Thank you for quick answer!

We will change our setup to force the uv cache folder to **.cache/uv_<uv_version>/**

Consider this one closed/resolved for our sake ;-)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-19 13:39_

---

_Comment by @charliermarsh on 2024-09-19 13:39_

We'll use this ticket to track improving the docs here.

---

_Closed by @charliermarsh on 2024-09-25 22:13_

---
