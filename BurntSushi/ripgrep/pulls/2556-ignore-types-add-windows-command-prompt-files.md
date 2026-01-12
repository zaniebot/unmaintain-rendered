```yaml
number: 2556
title: "ignore/types: add Windows Command Prompt files"
type: pull_request
state: merged
author: mataha
labels: []
assignees: []
merged: true
base: master
head: feature/ignore/types/cmd
created_at: 2023-07-09T19:15:55Z
updated_at: 2023-07-10T21:55:22Z
url: https://github.com/BurntSushi/ripgrep/pull/2556
synced_at: 2026-01-12T18:23:14Z
```

# ignore/types: add Windows Command Prompt files

---

_@mataha_

This PR adds `*.bat` and `*.cmd` file types.

In doing so, it makes a distinction between batch files (old standard from the MS-DOS era) and command scripts (new flavor - can operate on batch files, although `*.cmd` is preferred for various reasons, the main one being batch files will set `ERRORLEVEL` following inconsistent MS-DOS style rules).

---

_Review comment by @BurntSushi on `crates/ignore/src/default_types.rs`:27 on 2023-07-10 12:35_

Would `bat` be better here?

---

_@BurntSushi reviewed on 2023-07-10 12:36_

Is it possible to provide a source for this change? Ideally something from Microsoft's docs.

---

_Comment by @mataha on 2023-07-10 14:29_

> Is it possible to provide a source for this change? Ideally something from Microsoft's docs.

[This][1] is the only credible source though.

[1]: https://groups.google.com/g/microsoft.public.win2000.cmdprompt.admin/c/XHeUq8oe2wk/m/LIEViGNmkK0J#i106

---

_@mataha reviewed on 2023-07-10 14:31_

---

_Review comment by @mataha on `crates/ignore/src/default_types.rs`:27 on 2023-07-10 14:31_

May I suggest `bat` in addition to `batchfile`? As these are commonly called "batch files", even by Windows.

---

_@BurntSushi reviewed on 2023-07-10 14:33_

---

_Review comment by @BurntSushi on `crates/ignore/src/default_types.rs`:27 on 2023-07-10 14:33_

I suppose that's fine.

---

_Review comment by @BurntSushi on `crates/ignore/src/default_types.rs`:27 on 2023-07-10 14:33_

Well, I'd prefer `batch` and `bat`, not `batchfile`. `file` seems redundant in this case and no other file type uses `file` in the name.

---

_@BurntSushi reviewed on 2023-07-10 14:33_

---

_@mataha reviewed on 2023-07-10 14:35_

---

_Review comment by @mataha on `crates/ignore/src/default_types.rs`:27 on 2023-07-10 14:35_

`bat` and `batch` it is then.

---

_@BurntSushi approved on 2023-07-10 19:57_

Thanks!

---

_Merged by @BurntSushi on 2023-07-10 19:58_

---

_Closed by @BurntSushi on 2023-07-10 19:58_

---

_Branch deleted on 2023-07-10 21:55_

---
