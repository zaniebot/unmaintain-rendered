```yaml
number: 3181
title: "core: don't build decompression reader unless we intend to use it"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/skip-binary-resolution-windows
created_at: 2025-10-12T18:23:41Z
updated_at: 2025-10-12T20:31:22Z
url: https://github.com/BurntSushi/ripgrep/pull/3181
synced_at: 2026-01-12T18:23:15Z
```

# core: don't build decompression reader unless we intend to use it

---

_@BurntSushi_

Building it can consume resources. In particular, on Windows, the
various binaries are eagerly resolved.

I think this originally wasn't done. The eager resolution was added
later for security purposes. But the "eager" part isn't actually
necessary.

It would probably be better to change the decompression reader to do
lazy resolution only when the binary is needed. But this will at least
avoid doing anything when the `-z/--search-zip` flag isn't used. But
when it is, ripgrep will still eagerly resolve all possible binaries.

Fixes #2111


---

_Merged by @BurntSushi on 2025-10-12 20:31_

---

_Closed by @BurntSushi on 2025-10-12 20:31_

---

_Branch deleted on 2025-10-12 20:31_

---
