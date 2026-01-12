```yaml
number: 1100
title: Support Brotli and Zstd decompression
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: dana/brotli-zstd
created_at: 2018-11-03T21:08:06Z
updated_at: 2019-01-23T01:56:17Z
url: https://github.com/BurntSushi/ripgrep/pull/1100
synced_at: 2026-01-12T18:23:13Z
```

# Support Brotli and Zstd decompression

---

_@okdana_

Fixes #1099

I added Brotli because it feels kind of related in design, utility, and commonness to Zstd. Let me know if that was a bad choice

I also updated the ignore types. Not sure how useful it is to have those in there, but they were updated the last time a compression format was added, so...

---

_@BurntSushi approved on 2019-01-11 15:55_

Ah! Sorry @okdana, I missed this PR. :-)

---

_Comment by @BurntSushi on 2019-01-11 15:56_

(CI failure looks unrelated.)

---

_Comment by @okdana on 2019-01-11 16:01_

This one does lack the `.zstd` extension that @chipturner included, which seems like a good addition. If we do go with mine i should probably add that in

---

_Comment by @BurntSushi on 2019-01-11 16:03_

@okdana Ah yeah, that'd be great!

---

_Comment by @okdana on 2019-01-11 21:12_

Updated and rebased

---

_Merged by @BurntSushi on 2019-01-23 01:56_

---

_Closed by @BurntSushi on 2019-01-23 01:56_

---
