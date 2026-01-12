```yaml
number: 2712
title: "uv-cache: bump simple cache version"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
assignees: []
merged: true
base: main
head: ag/bump-cache
created_at: 2024-03-28T17:18:50Z
updated_at: 2024-03-28T17:29:30Z
url: https://github.com/astral-sh/uv/pull/2712
synced_at: 2026-01-12T16:05:11Z
```

# uv-cache: bump simple cache version

---

_@BurntSushi_

It seems likely that we forgot to bump the version of the "simple" cache
in the 0.1.25 release. I'm still working on confirming it, but I figured
I'd get this bump up first.

The main problem here is that our "simple" cache is represented by
`rkyv`, and that in turn is tightly coupled to the representation of a
selection of data types in `uv`. Changing those data types without
bumping the cache version can result in cache deserialization errors
like this, or in the worst case, silent logic errors.

One possibility here is that the representation changed in a way that
permitted it to pass `rkyv` validation, but changed how the data itself
is interpreted. Our cache is robust with respect to `rkyv` validation
(if it fails, the cache will invalidate the entry and self-heal), but
being robust to higher level logical errors in interpretation of the
data is a much more significant challenge. Our best bet there is perhaps
some kind of checksum that we could do on top of `rkyv` validation (or
instead of it), and thus convert silent logical changes in how the data
is interpreted into failure modes that we're already robust to.

Fixes #2711


---

_Review requested from @charliermarsh by @BurntSushi on 2024-03-28 17:19_

---

_Review requested from @zanieb by @BurntSushi on 2024-03-28 17:19_

---

_@zanieb approved on 2024-03-28 17:29_

---

_Label `bug` added by @zanieb on 2024-03-28 17:29_

---

_Merged by @zanieb on 2024-03-28 17:29_

---

_Closed by @zanieb on 2024-03-28 17:29_

---

_Branch deleted on 2024-03-28 17:29_

---
