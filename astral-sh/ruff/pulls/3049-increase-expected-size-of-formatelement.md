```yaml
number: 3049
title: Increase expected size of FormatElement
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/target-size
created_at: 2023-02-20T04:31:24Z
updated_at: 2023-02-21T07:59:10Z
url: https://github.com/astral-sh/ruff/pull/3049
synced_at: 2026-01-12T15:55:12Z
```

# Increase expected size of FormatElement

---

_@charliermarsh_

It looks like this is now `32usize` due to `StaticTextSlice`. We should revisit this.

---

_Comment by @charliermarsh on 2023-02-20 04:31_

\cc @MichaReiser 

---

_Merged by @charliermarsh on 2023-02-20 17:47_

---

_Closed by @charliermarsh on 2023-02-20 17:47_

---

_Branch deleted on 2023-02-20 17:47_

---

_Comment by @MichaReiser on 2023-02-21 07:59_

This is unfortunate, but I don't see another way of modeling the `TextSlice` without implementing `Rc` manually (that does not have a weak reference counter). The good news is that shrinking the `FormatElement` size from 32 to 24 bytes wasn't as big a performance improvement as bringing it down to 32. 

---
