```yaml
number: 2312
title: bump bstr to 1.0
type: pull_request
state: closed
author: pascalkuthe
labels: []
assignees: []
base: master
head: bstr-update
created_at: 2022-09-23T18:54:45Z
updated_at: 2023-03-04T03:23:28Z
url: https://github.com/BurntSushi/ripgrep/pull/2312
synced_at: 2026-01-12T18:23:14Z
```

# bump bstr to 1.0

---

_@pascalkuthe_

updates bstr to 1.0 and closes #2310
This allows downstream crates to use bstr 1.0 (or other crates that depend on it) without compiling it twice.

This raises the MSRV to 1.60.
When I first filed #2310 I assumed that raising the MSRV was not an option because it was set to 1.34.
However I discovered that ripgrep already has an MSRV of 1.52 since 9f924ee187d4c62aa6ebe4903d0cfc6507a5adb5.
To avoid future confusions like this I also updated the README with the new MSRV.


---

_Comment by @stmcginnis on 2022-11-10 20:25_

This would be helpful for the project I'm working on to get this. Anything we can do to help get this PR through?

---

_Comment by @pascalkuthe on 2023-03-04 03:11_

This is actually redundant now as @BurntSushi fixed this himself, glad to see this land

---

_Closed by @pascalkuthe on 2023-03-04 03:11_

---

_Comment by @BurntSushi on 2023-03-04 03:19_

Aye yeah, thanks. Sorry I missed this PR. Generally speaking, I do all dependency upgrades myself so that I can scrutinize changes to the dependency tree.

---

_Comment by @pascalkuthe on 2023-03-04 03:23_

Fair enough, I assumed this was just an uncontroversial chore since you are also the author of `bstr` :D Thanks for all your work :)

---

_Branch deleted on 2023-03-04 03:23_

---
