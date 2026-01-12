```yaml
number: 1116
title: "ignore crate: Show matched FileTypeDef?"
type: issue
state: closed
author: firesock
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2018-11-21T00:25:00Z
updated_at: 2019-01-24T01:09:15Z
url: https://github.com/BurntSushi/ripgrep/issues/1116
synced_at: 2026-01-12T16:13:22Z
```

# ignore crate: Show matched FileTypeDef?

---

_@firesock_

#### What version of ripgrep are you using?

ignore-0.4.4

#### How did you install ripgrep?

ignore crate via crates.io

#### What operating system are you using ripgrep on?

Linux

#### Describe your question, feature request, or bug.

Is it feasible to make ignore's `Walk` return `DirEntry`'s that are aware of the `FileTypeDef` they matched on, if any?

My use case is to use `Walk` to iterate and depending on the name registered with the matching `FileTypeDef` do some different logic.

As far as I can tell, this information isn't accessible easily right now. Even if I used `Types` and called `matched`, the `Glob` struct doesn't return the `FileTypeDef` and hence the name registered.

---

_Comment by @BurntSushi on 2018-11-21 00:29_

It should be possible to expose by adding a `file_type_def` method on `ignore::types::Glob` that returns a `Option<&FileTypeDef>`. It is already part of the type: https://docs.rs/ignore/0.4.4/src/ignore/types.rs.html#334

---

_Label `enhancement` added by @BurntSushi on 2018-11-21 00:29_

---

_Label `help wanted` added by @BurntSushi on 2018-11-21 00:29_

---

_Comment by @firesock on 2018-11-21 00:38_

Okay thanks, is the intended goal to use `Types` and `matched` after a `DirEntry` is returned, and not expose it on `DirEntry` at all?

---

_Comment by @BurntSushi on 2018-11-21 00:51_

If you can make that work, then I think that would be ideal. Exposing it through the directory iterator (i.e., `DirEntry`) would require more API design work.

---

_Closed by @BurntSushi on 2019-01-24 01:09_

---
