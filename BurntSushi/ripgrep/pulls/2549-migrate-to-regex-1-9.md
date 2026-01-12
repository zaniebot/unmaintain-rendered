```yaml
number: 2549
title: migrate to regex 1.9
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/regex-automata
created_at: 2023-07-05T15:12:55Z
updated_at: 2023-07-05T18:04:33Z
url: https://github.com/BurntSushi/ripgrep/pull/2549
synced_at: 2026-01-12T18:23:14Z
```

# migrate to regex 1.9

---

_@BurntSushi_

This includes slimming down the `grep-regex` crate substantially now that `regex-automata 0.3` is available. This also includes rewriting the inner literal extractor. The new extractor can now pluck literals out of more complex regexes such as `\s+(Sherlock|[A-Z]atso[a-z]|Moriarty)\s+`. The current version of ripgrep doesn't do any literal optimizations for that regex in particular.

Interestingly, this PR actually marks the end of ripgrep depending on `regex` directly for its primary search work load. It now depends on `regex-automata` directly, which exposes a more expressive API. For example, it lets one build regexes directly from `Hir`, which is quite useful in ripgrep's case because of the transformations it does on `Hir`. Previously, we had to roundtrip `Hir` back to the concrete syntax in order to turn it into a `regex::Regex`, since the `regex` crate very intentionally has no public dependency on `regex-syntax`. But `regex-automata`, as a more internal library that can evolve more rapidly, does have a public dependency on `regex-syntax`.

---

_Merged by @BurntSushi on 2023-07-05 18:04_

---

_Closed by @BurntSushi on 2023-07-05 18:04_

---

_Branch deleted on 2023-07-05 18:04_

---
