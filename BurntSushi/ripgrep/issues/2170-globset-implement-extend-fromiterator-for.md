```yaml
number: 2170
title: "Globset: Implement Extend, FromIterator for GlobSetBuilder"
type: issue
state: open
author: ColonelThirtyTwo
labels: []
assignees: []
created_at: 2022-03-31T14:09:56Z
updated_at: 2023-10-17T07:23:49Z
url: https://github.com/BurntSushi/ripgrep/issues/2170
synced_at: 2026-01-12T16:13:24Z
```

# Globset: Implement Extend, FromIterator for GlobSetBuilder

---

_@ColonelThirtyTwo_

Seems like easy traits to add, and would make creating a GlobSet from a vector of globs more ergonomic.

---

_Comment by @jplatte on 2023-10-17 07:23_

`FromIterator` seems like it could also be implemented by `GlobSet` itself. I don't think it's very common for builders to implement one of these traits. 

---
