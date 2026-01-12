```yaml
number: 3071
title: Add filter for install_registry_source_dist_cached on Gentoo
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/gentoo
created_at: 2024-04-16T18:18:50Z
updated_at: 2024-04-16T19:07:49Z
url: https://github.com/astral-sh/uv/pull/3071
synced_at: 2026-01-12T16:05:24Z
```

# Add filter for install_registry_source_dist_cached on Gentoo

---

_@charliermarsh_

Closes https://github.com/astral-sh/uv/issues/3051.

---

_Marked ready for review by @charliermarsh on 2024-04-16 18:18_

---

_Review requested from @zanieb by @charliermarsh on 2024-04-16 18:18_

---

_Label `testing` added by @charliermarsh on 2024-04-16 18:18_

---

_Comment by @mgorny on 2024-04-16 18:51_

Any clue why I would be getting a different number? I'm sorry but I don't really know what that test is doing.

---

_Comment by @charliermarsh on 2024-04-16 18:53_

I'm not totally sure. That's basically the "number of files produced when building the wheel", because it's a count of the number of files in the cache. We could just filter out the exact number entirely, but it's kind of useful to know when it changes.

---

_@zanieb approved on 2024-04-16 19:00_

---

_Merged by @charliermarsh on 2024-04-16 19:07_

---

_Closed by @charliermarsh on 2024-04-16 19:07_

---

_Branch deleted on 2024-04-16 19:07_

---
