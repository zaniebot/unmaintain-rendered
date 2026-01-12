```yaml
number: 3021
title: Update decompress.rs
type: pull_request
state: closed
author: bcling
labels: []
assignees: []
base: master
head: master
created_at: 2025-04-02T00:09:50Z
updated_at: 2025-04-08T15:52:46Z
url: https://github.com/BurntSushi/ripgrep/pull/3021
synced_at: 2026-01-12T18:23:14Z
```

# Update decompress.rs

---

_@bcling_

gzip suffix support added. Seems to work for me.

---

_Comment by @ltrzesniewski on 2025-04-02 08:33_

Shouldn't that also be added to `default_types.rs` to make `-tgzip` work?

---

_Comment by @bcling on 2025-04-02 11:24_

> Shouldn't that also be added to `default_types.rs` to make `-tgzip` work?

It was already in there:

```
(&["gzip"], &["*.gz", "*.tgz"]),
```

---

_Comment by @ltrzesniewski on 2025-04-02 11:42_

It's in the key, I meant to add it to the value:

```
(&["gzip"], &["*.gz", "*.gzip", "*.tgz"]),
```

---

_Comment by @bcling on 2025-04-03 00:04_

You were very clear, I failed my code reading comprehension. Added as an additional value as noted above.

---

_Review comment by @ltrzesniewski on `crates/ignore/src/default_types.rs`:100 on 2025-04-03 08:24_

The `*` is missing:

```rust
(&["gzip"], &["*.gz", "*.gzip", "*.tgz"]),
```

---

_@ltrzesniewski reviewed on 2025-04-03 08:26_

---

_Comment by @bcling on 2025-04-03 16:13_

Ugh. Apologies for making you proof what I should have caught. Thanks for the gentle corrections.

---

_Comment by @BurntSushi on 2025-04-08 15:52_

See https://github.com/BurntSushi/ripgrep/issues/3020#issuecomment-2786892024

---

_Closed by @BurntSushi on 2025-04-08 15:52_

---
