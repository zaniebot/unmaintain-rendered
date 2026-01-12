```yaml
number: 857
title: update regex to 0.2.9, enables portable binary releases
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/update-regex
created_at: 2018-03-13T03:05:03Z
updated_at: 2018-03-13T03:21:46Z
url: https://github.com/BurntSushi/ripgrep/pull/857
synced_at: 2026-01-12T18:23:13Z
```

# update regex to 0.2.9, enables portable binary releases

---

_@BurntSushi_

Pretty much what it says on the tin. This also brings in new AVX2 optimizations added in the regex crate. See: https://github.com/rust-lang/regex/pull/456

We also employ the totally novel idea of updating the CHANGELOG incrementally, instead of doing everything in one swoop during a release.

Note that this does not move to `regex-syntax 0.5.0` yet. That requires a bit more work.

---

_Merged by @BurntSushi on 2018-03-13 03:21_

---

_Closed by @BurntSushi on 2018-03-13 03:21_

---

_Branch deleted on 2018-03-13 03:21_

---
