```yaml
number: 2648
title: rollup PR queue + drop asciidoc/asciidoctor + drop serde_derive + drop base64
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/rollup
created_at: 2023-11-21T23:35:17Z
updated_at: 2023-11-21T23:39:36Z
url: https://github.com/BurntSushi/ripgrep/pull/2648
synced_at: 2026-01-12T18:23:14Z
```

# rollup PR queue + drop asciidoc/asciidoctor + drop serde_derive + drop base64

---

_@BurntSushi_

Most of this PR is focused on dropping dependencies that are probably not carrying their weight. This gets us back down to the size of the dependency tree in `ripgrep 0.3.2`, but the compile times are still a couple seconds slower for a clean release build (without PCRE2).

At this point, `cargo build --release --timings` on a clean build suggests that the biggest contributor to compile time is `regex-automata`. That's no surprise.

---

_Merged by @BurntSushi on 2023-11-21 23:39_

---

_Closed by @BurntSushi on 2023-11-21 23:39_

---

_Branch deleted on 2023-11-21 23:39_

---
