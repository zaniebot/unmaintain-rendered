```yaml
number: 1986
title: "The ignore crate probably shouldn't special-case the path \"-\""
type: issue
state: open
author: tavianator
labels: []
assignees: []
created_at: 2021-09-09T19:44:06Z
updated_at: 2021-09-09T19:44:06Z
url: https://github.com/BurntSushi/ripgrep/issues/1986
synced_at: 2026-01-12T16:13:24Z
```

# The ignore crate probably shouldn't special-case the path "-"

---

_@tavianator_

Currently `ignore` handles the literal path `-` as meaning stdin: https://github.com/BurntSushi/ripgrep/blob/9b01a8f9ae53ebcd05c27ec21843758c2c1e823f/crates/ignore/src/walk.rs#L1241

This is probably desirable for ripgrep but other users of `ignore` (such as [fd](https://github.com/sharkdp/fd/issues/849)) may not want the special treatment.  Can the special casing be moved into the ripgrep front end?

I'm happy to do the implementation work if you agree and point me to the right place in the code.

---
