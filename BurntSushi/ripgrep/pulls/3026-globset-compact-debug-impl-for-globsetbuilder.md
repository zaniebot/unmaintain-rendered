```yaml
number: 3026
title: "globset: compact Debug impl for GlobSetBuilder"
type: pull_request
state: closed
author: squidfunk
labels:
  - rollup
assignees: []
base: master
head: feature/debug-impl-set-builder
created_at: 2025-04-09T19:25:09Z
updated_at: 2025-09-20T01:08:33Z
url: https://github.com/BurntSushi/ripgrep/pull/3026
synced_at: 2026-01-12T18:23:14Z
```

# globset: compact Debug impl for GlobSetBuilder

---

_@squidfunk_

Supersedes #3025, as discussed in https://github.com/BurntSushi/ripgrep/pull/3025#issuecomment-2790663355 and https://github.com/BurntSushi/ripgrep/pull/3025#issuecomment-2790741277.

---

_Label `rollup` added by @BurntSushi on 2025-07-27 14:30_

---

_Comment by @BurntSushi on 2025-07-27 14:33_

Thanks! This wasn't quite what I would like. The allocs in the `Debug` impl are somewhat unfortunate. Instead, I implemented a more concise `Debug` impl on `Glob` and then used that in `GlobSetBuilder`.

Ideally we'd have a concise `Debug` impl for `GlobSet` too, but that's harder. And involves some design trade-offs since the original glob patterns are dropped by the time it is constructed.

---

_Comment by @squidfunk on 2025-07-27 15:19_

Thanks for coming back to this PR! I don't really mind how things are implemented, but more concise debug output would be helpful, alternatively implementing `IntoIterator` as I proposed in https://github.com/BurntSushi/ripgrep/pull/3025, which would allow to customize debug outputs.

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
