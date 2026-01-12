```yaml
number: 622
title: "Add message to README to inform about `cargo install ripgrep` not being `strip`ed"
type: pull_request
state: merged
author: dvergeylen
labels: []
assignees: []
merged: true
base: master
head: dvergeylen-595
created_at: 2017-10-03T15:04:25Z
updated_at: 2017-10-08T12:01:30Z
url: https://github.com/BurntSushi/ripgrep/pull/622
synced_at: 2026-01-12T18:23:13Z
```

# Add message to README to inform about `cargo install ripgrep` not being `strip`ed

---

_@dvergeylen_

Notify user cargo install ripgrep contains debug symbols and informs how to stripe them.

Fix #595 

---

_Renamed from "Fix #595" to "Add message to README to inform about `cargo install ripgrep` not being `strip`ed" by @dvergeylen on 2017-10-03 15:09_

---

_Review comment by @BurntSushi on `README.md`:213 on 2017-10-03 15:25_

Please format the README in a style that is consistent with the rest of the text. In this case, please wrap text to 79 columns inclusive.

---

_Review comment by @BurntSushi on `README.md`:214 on 2017-10-03 15:26_

Please reword this to:

> Note that the binary may be bigger than expected because it contains debug symbols. This is intentional. To remove debug symbols and therefore reduce the file size, run `strip` on the binary.

---

_@BurntSushi requested changes on 2017-10-03 15:26_

Thanks! I appreciate this PR. I've made a nit and requested rewording.

---

_@BurntSushi approved on 2017-10-03 15:43_

Thanks!

---

_Merged by @BurntSushi on 2017-10-08 12:01_

---

_Closed by @BurntSushi on 2017-10-08 12:01_

---
