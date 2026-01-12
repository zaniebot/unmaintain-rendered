```yaml
number: 2614
title: lots of little polishing
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/polish
created_at: 2023-09-26T19:05:46Z
updated_at: 2023-10-10T00:29:56Z
url: https://github.com/BurntSushi/ripgrep/pull/2614
synced_at: 2026-01-12T18:23:14Z
```

# lots of little polishing

---

_@BurntSushi_

This PR brings a lot of the code into line with my current style, which has slightly shifted over the years. I've also dropped dependencies where possible. For example, `globset` no longer depends on the `fnv` crate, and instead just rolls the FNV hashing itself. Similarly, the `regex` crate is dropped as a dependency entirely, and we instead depend on `regex-automata` directly. This leads to the interesting result that ripgrep no longer depends on `regex` _anywhere_ in its tree(!), but it is still most convenient to say that it uses the regex crate when talking about the kind of regex flavor it supports.

---

_@ltrzesniewski reviewed on 2023-09-26 19:16_

---

_Review comment by @ltrzesniewski on `crates/core/app.rs`:1592 on 2023-09-26 19:16_

```suggestion
in the output. To make the path appear, and thus also a hyperlink, use the
```

😅 

---

_Merged by @BurntSushi on 2023-10-10 00:29_

---

_Closed by @BurntSushi on 2023-10-10 00:29_

---

_Branch deleted on 2023-10-10 00:29_

---
