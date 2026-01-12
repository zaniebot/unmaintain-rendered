```yaml
number: 2369
title: "globset: introduce option to keep empty alternates"
type: pull_request
state: closed
author: zombiepigdragon
labels:
  - rollup
assignees: []
base: master
head: globset-empty-alternate
created_at: 2022-12-10T22:57:48Z
updated_at: 2023-07-08T22:52:56Z
url: https://github.com/BurntSushi/ripgrep/pull/2369
synced_at: 2026-01-12T18:23:14Z
```

# globset: introduce option to keep empty alternates

---

_@zombiepigdragon_

Add a method `GlobBuilder::empty_alternates` and supporting mechanisms. The corresponding option was left off by default to maintain backwards compatibility.

Partially resolves BurntSushi/ripgrep#1368. The issue mentions ripgrep in particular, while this PR only works with globset in general, and leaves enabling the setting in ripgrep open to a future decision.

---

_@BurntSushi approved on 2023-07-07 18:50_

Thanks, LGTM.

I hope to do a `globset` overhaul at some point, and I wonder whether it makes sense to just have this unconditionally enabled without exposing any option. I suspect I originally did things this way because the underlying regex engine couldn't handle empty alternates. But it can now.

---

_Label `rollup` added by @BurntSushi on 2023-07-07 18:50_

---

_Closed by @BurntSushi on 2023-07-08 22:52_

---
