```yaml
number: 3592
title: "Move `extras` onto annotated distribution"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/extras
created_at: 2024-05-14T20:51:37Z
updated_at: 2024-05-14T23:25:07Z
url: https://github.com/astral-sh/uv/pull/3592
synced_at: 2026-01-12T16:05:44Z
```

# Move `extras` onto annotated distribution

---

_@charliermarsh_

## Summary

Like hashes, we can now store these on `AnnotatedDist` rather than creating a parallel hash map and performing lookups later on.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-14 20:51_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-05-14 20:51_

---

_Label `internal` added by @charliermarsh on 2024-05-14 20:51_

---

_Marked ready for review by @charliermarsh on 2024-05-14 20:51_

---

_@BurntSushi approved on 2024-05-14 22:34_

This makes sense to me.

I think that this data is different from hashes and `Metadata23` in that it isn't attached to a specific distribution, but rather, is a property of both the distribution and the dependency specification. So unlike hashes/metadata, I think extras _can't_ be on the lower level distribution types, right? (I'm like 80% confident in this.)

---

_Comment by @charliermarsh on 2024-05-14 23:08_

Yeah that's correct.

---

_Merged by @charliermarsh on 2024-05-14 23:25_

---

_Closed by @charliermarsh on 2024-05-14 23:25_

---

_Branch deleted on 2024-05-14 23:25_

---
