```yaml
number: 1054
title: Ripgrep is also available in Ubuntu (from Cosmic)
type: pull_request
state: merged
author: sylvestre
labels: []
assignees: []
merged: true
base: master
head: patch-2
created_at: 2018-09-14T06:41:26Z
updated_at: 2018-09-14T15:44:41Z
url: https://github.com/BurntSushi/ripgrep/pull/1054
synced_at: 2026-01-12T18:23:13Z
```

# Ripgrep is also available in Ubuntu (from Cosmic)

---

_@sylvestre_

_No description provided._

---

_@BurntSushi reviewed on 2018-09-14 13:31_

---

_Review comment by @BurntSushi on `README.md`:306 on 2018-09-14 13:31_

Oh interesting. What is the difference between the `ripgrep` and `rust-ripgrep` packages?

---

_Merged by @BurntSushi on 2018-09-14 13:32_

---

_Closed by @BurntSushi on 2018-09-14 13:32_

---

_Comment by @BurntSushi on 2018-09-14 13:32_

woohooo! Thanks @sylvestre!

---

_Comment by @est31 on 2018-09-14 14:09_

Nice, just thought today to check again on whether ripgrep will arrive in the next ubuntu or not. Thanks @sylvestre and @infinity0 ! Looking forward to the Ubuntu cosmic release.

---

_Comment by @sylvestre on 2018-09-14 14:10_

rust-ripgrep is the name of the source package.  We are prefixing all rust packages to avoid conflicts in the naming (example rust-num). This is usually transparent for the user.
ripgrep is the name of the binary package.

---

_@sdroege reviewed on 2018-09-14 15:44_

---

_Review comment by @sdroege on `README.md`:306 on 2018-09-14 15:44_

The source package is called `rust-ripgrep`, the binary package is called `ripgrep`

---
