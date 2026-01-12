```yaml
number: 319
title: Fix invalid UTF-8 output on Windows.
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: fix-318
created_at: 2017-01-14T03:46:48Z
updated_at: 2017-01-14T04:21:43Z
url: https://github.com/BurntSushi/ripgrep/pull/319
synced_at: 2026-01-12T18:23:12Z
```

# Fix invalid UTF-8 output on Windows.

---

_@BurntSushi_

Currently, Rust's standard library will yield an error if one tries to
write invalid UTF-8 to a Windows console. This is problematic for
ripgrep when it tries to print matched lines that contain invalid UTF-8.
We work around this by modifying the `termcolor` library to lossily
decode UTF-8 before sending it to `std::io::Stdout`. This may result in
some Unicode replacement chars being shown in the output, but this is
strictly better than erroring or not showing anything at all.

Fixes #318.

---

_Merged by @BurntSushi on 2017-01-14 04:21_

---

_Closed by @BurntSushi on 2017-01-14 04:21_

---

_Branch deleted on 2017-01-14 04:21_

---
