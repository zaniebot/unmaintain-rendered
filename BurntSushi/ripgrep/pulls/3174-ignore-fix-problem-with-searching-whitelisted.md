```yaml
number: 3174
title: "ignore: fix problem with searching whitelisted hidden files"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/fix-3173
created_at: 2025-10-09T01:11:05Z
updated_at: 2025-10-09T01:17:00Z
url: https://github.com/BurntSushi/ripgrep/pull/3174
synced_at: 2026-01-12T18:23:15Z
```

# ignore: fix problem with searching whitelisted hidden files

---

_@BurntSushi_

... specifically, when the whitelist comes from a _parent_ gitignore
file.

Our handling of parent gitignores is pretty ham-fisted and has been a
source of some unfortunate bugs. The problem is that we need to strip
the parent path from the path we're searching in order to correctly
apply the globs. But getting this stripping correct seems to be a subtle
affair.

Fixes #3173


---

_Merged by @BurntSushi on 2025-10-09 01:16_

---

_Closed by @BurntSushi on 2025-10-09 01:16_

---

_Branch deleted on 2025-10-09 01:17_

---
