```yaml
number: 768
title: update bytecount to improve perf with current rustc
type: pull_request
state: merged
author: llogiq
labels: []
assignees: []
merged: true
base: master
head: bytecount-0.3.1
created_at: 2018-01-30T20:57:48Z
updated_at: 2018-01-30T23:02:58Z
url: https://github.com/BurntSushi/ripgrep/pull/768
synced_at: 2026-01-12T18:23:13Z
```

# update bytecount to improve perf with current rustc

---

_@llogiq_

This incorporates the simplification @Veedrac made, because rustc apparently changed codegen. It does *not* include any additional dependencies.

---

_Comment by @BurntSushi on 2018-01-30 21:00_

@llogiq Thanks! I think you probably want to run `cargo update -p bytecount` as well in order to bump the lockfile.

---

_Comment by @llogiq on 2018-01-30 21:02_

Thanks, amended.

---

_@BurntSushi approved on 2018-01-30 21:07_

---

_Merged by @BurntSushi on 2018-01-30 21:34_

---

_Closed by @BurntSushi on 2018-01-30 21:34_

---

_Branch deleted on 2018-01-30 23:02_

---
