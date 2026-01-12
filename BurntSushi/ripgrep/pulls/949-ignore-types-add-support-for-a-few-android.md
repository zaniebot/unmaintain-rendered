```yaml
number: 949
title: "ignore/types: add support for a few Android platform file types"
type: pull_request
state: merged
author: amhk
labels: []
assignees: []
merged: true
base: master
head: android-file-types
created_at: 2018-06-15T11:31:07Z
updated_at: 2018-06-18T06:52:52Z
url: https://github.com/BurntSushi/ripgrep/pull/949
synced_at: 2026-01-12T18:23:13Z
```

# ignore/types: add support for a few Android platform file types

---

_@amhk_

Add the following file types to the built-in type list:

- Android AIDL files (*.aidl)
- Android platform makefiles (*.mk, *.bp)

Note: I went with "amake" as the name for the Android makefile pattern because it's quick to write. One could argue that something like "android-make" is easier to comprehend when skimming through the output of --type-list.

---

_@BurntSushi approved on 2018-06-15 12:05_

Thanks!

Also, can I just say, it makes me unreasonably happy to see you following my commit message conventions. :-)

---

_Comment by @amhk on 2018-06-15 12:10_

>>Also, can I just say, it makes me unreasonably happy to see you following my commit message conventions. :-)

You're welcome! Thanks for creating and maintaining a great tool.

---

_Merged by @BurntSushi on 2018-06-15 12:29_

---

_Closed by @BurntSushi on 2018-06-15 12:29_

---

_Branch deleted on 2018-06-18 06:52_

---
