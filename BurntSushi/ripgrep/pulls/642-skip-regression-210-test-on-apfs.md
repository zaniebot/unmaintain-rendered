```yaml
number: 642
title: Skip regression 210 test on APFS
type: pull_request
state: merged
author: sebnow
labels: []
assignees: []
merged: true
base: master
head: apfs_regression_210
created_at: 2017-10-16T09:48:59Z
updated_at: 2017-10-21T00:51:14Z
url: https://github.com/BurntSushi/ripgrep/pull/642
synced_at: 2026-01-12T18:23:13Z
```

# Skip regression 210 test on APFS

---

_@sebnow_

APFS does not support creating filenames with invalid UTF-8 byte codes,
thus this test doesn't make sense. Skip it on file systems where this
shouldn't be possible.

Fixes #559

---

_Review comment by @BurntSushi on `tests/workdir.rs`:44 on 2017-10-16 10:54_

Please wrap this (and other comments) to 79 columns inclusive. Thanks!

---

_@BurntSushi requested changes on 2017-10-16 10:54_

Looks great! Thanks. One style nit and this should be good to go.

---

_@BurntSushi approved on 2017-10-16 13:53_

Awesome, thanks! :-)

---

_Merged by @BurntSushi on 2017-10-21 00:51_

---

_Closed by @BurntSushi on 2017-10-21 00:51_

---
