```yaml
number: 3166
title: "printer: finish removal of `max_matches`"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/finish-max-matches-refactor
created_at: 2025-10-04T12:57:42Z
updated_at: 2025-10-04T13:19:55Z
url: https://github.com/BurntSushi/ripgrep/pull/3166
synced_at: 2026-01-12T18:23:15Z
```

# printer: finish removal of `max_matches`

---

_@BurntSushi_

This finishes what I started in commit
a6e0be3c909c5c09e5fb402c907f3beb88cfb4c4.
Specifically, the `max_matches` configuration has been moved to the
`grep-searcher` crate and *removed* from the `grep-printer` crate. The
commit message has the details for why we're doing this, but the short
story is to fix #3076 without other regressions.

Note that this is a breaking change for `grep-printer`, so this will
require a semver incompatible release.


---

_Merged by @BurntSushi on 2025-10-04 13:19_

---

_Closed by @BurntSushi on 2025-10-04 13:19_

---

_Branch deleted on 2025-10-04 13:19_

---
