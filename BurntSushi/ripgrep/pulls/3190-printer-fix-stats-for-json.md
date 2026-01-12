```yaml
number: 3190
title: "printer: fix `--stats` for `--json`"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/fix-json-stats
created_at: 2025-10-16T01:07:31Z
updated_at: 2025-10-16T01:21:22Z
url: https://github.com/BurntSushi/ripgrep/pull/3190
synced_at: 2026-01-12T18:23:15Z
```

# printer: fix `--stats` for `--json`

---

_@BurntSushi_

Somehow, the JSON printer seems to have never emitted correct summary
statistics. And I believe #3178 is the first time anyone has ever
reported it. I believe this bug has persisted for years. That's
surprising.

Anyway, the problem here was that we were bailing out of `finish()` on
the sink if we weren't supposed to print anything. But we bailed out
before we tallied our summary statistics. Obviously we shouldn't do
that.

Fixes #3178


---

_Merged by @BurntSushi on 2025-10-16 01:21_

---

_Closed by @BurntSushi on 2025-10-16 01:21_

---

_Branch deleted on 2025-10-16 01:21_

---
