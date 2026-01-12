```yaml
number: 1613
title: On Windows MSVC, statically link the C runtime
type: pull_request
state: closed
author: AustinWise
labels:
  - rollup
assignees: []
base: master
head: austin/StaticlyLinkMsvcrt
created_at: 2020-06-09T21:29:23Z
updated_at: 2022-02-21T22:37:45Z
url: https://github.com/BurntSushi/ripgrep/pull/1613
synced_at: 2026-01-12T18:23:14Z
```

# On Windows MSVC, statically link the C runtime

---

_@AustinWise_

Before this change, rg.exe depended on vcruntime140.dll, which does not exist on a fresh install of Windows. The missing DLL prevents rg.exe from starting.


---

_Comment by @AustinWise on 2020-06-09 21:30_

There is precedence for using this option. The `rg.exe` shipped with VS Code is [compiled with this option](https://github.com/microsoft/ripgrep-prebuilt/blob/677ffbed25585367af2d7642650f976496091a23/build/windows.yml#L35). rustc.exe is [compiled with this option](https://github.com/rust-lang/rust/pull/39837) too.


---

_Comment by @AustinWise on 2020-06-09 23:22_

The main downside to this change is it grows the binary size. For x64, it increases 2% from 4.14 MB to 4.24 MB.

---

_Comment by @wdscxsj on 2020-09-01 08:22_

I suppose 0.1 MB is negligible here... An immediately usable binary is much better than having to find and download the venerable Visual C++ Redistributable in emergency.

---

_@BurntSushi approved on 2021-05-31 00:26_

I love this, thank you! I'll also update the README which IIRC mentions installing vcruntime or similar.

---

_Label `rollup` added by @BurntSushi on 2021-05-31 00:28_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---

_Branch deleted on 2022-02-21 22:37_

---
