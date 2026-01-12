```yaml
number: 531
title: add --iglob flag
type: pull_request
state: merged
author: PeterSP
labels: []
assignees: []
merged: true
base: master
head: add_iglob_flag
created_at: 2017-06-29T02:59:22Z
updated_at: 2017-07-03T14:23:20Z
url: https://github.com/BurntSushi/ripgrep/pull/531
synced_at: 2026-01-12T18:23:13Z
```

# add --iglob flag

---

_@PeterSP_

Working with Chris Stadler, implemented
https://github.com/BurntSushi/ripgrep/issues/163#issuecomment-300012592

This addresses #163 

---

_@PeterSP reviewed on 2017-06-29 03:01_

---

_Review comment by @PeterSP on `tests/tests.rs`:348 on 2017-06-29 03:01_

This is `file1.HTML` &`file2.html` and not `file.HTML` & `file.html` because the test is intended to create two distinct files and that would not work in the latter case on a case-insensitive filesystem such as that of Mac OS X or Windows.

---

_Merged by @BurntSushi on 2017-07-03 10:52_

---

_Closed by @BurntSushi on 2017-07-03 10:52_

---

_Comment by @BurntSushi on 2017-07-03 10:53_

This looks like great work! Thanks @PeterSP and Chris!

---

_Branch deleted on 2017-07-03 14:23_

---
