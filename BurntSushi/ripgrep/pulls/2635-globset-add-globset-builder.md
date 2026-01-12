```yaml
number: 2635
title: "globset: Add GlobSet::builder"
type: pull_request
state: closed
author: jplatte
labels:
  - rollup
assignees: []
base: master
head: globset-builder
created_at: 2023-10-16T21:57:25Z
updated_at: 2023-11-21T04:51:57Z
url: https://github.com/BurntSushi/ripgrep/pull/2635
synced_at: 2026-01-12T18:23:14Z
```

# globset: Add GlobSet::builder

---

_@jplatte_

It's a somewhat common pattern to have `Type::builder` be an alias for `TypeBuilder::new` (or even the only way of constructing `TypeBuilder`). It means you don't have to import the builder type separately for common usage.

Thanks for this very handy small library 💚 

---

_@BurntSushi approved on 2023-10-16 22:05_

Thanks! I've taken to doing this in other crates as well. I like it.

---

_Label `rollup` added by @BurntSushi on 2023-10-16 22:11_

---

_Comment by @jplatte on 2023-11-06 12:54_

I'm not in a hurry to have this merged, but is it normal that it takes many weeks from creation of rollup to merge?

---

_Closed by @BurntSushi on 2023-11-21 04:51_

---
