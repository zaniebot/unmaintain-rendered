```yaml
number: 867
title: Fix bold, intense color on Windows 10 console.
type: pull_request
state: merged
author: ehuss
labels: []
assignees: []
merged: true
base: master
head: windows-intense-bold-fix
created_at: 2018-03-26T02:59:57Z
updated_at: 2018-03-26T20:42:54Z
url: https://github.com/BurntSushi/ripgrep/pull/867
synced_at: 2026-01-12T18:23:13Z
```

# Fix bold, intense color on Windows 10 console.

---

_@ehuss_

There is an issue with the Windows 10 console where if you issue the bold
escape sequence after one of the extended foreground colors, it overrides the
color.  This happens in termcolor if you have bold, intense, and color set.
The workaround is to issue the bold sequence before the color.

This is for rust-lang/rust#49322.

---

_Merged by @BurntSushi on 2018-03-26 20:42_

---

_Closed by @BurntSushi on 2018-03-26 20:42_

---

_Comment by @BurntSushi on 2018-03-26 20:42_

Awesome, thank you!

---
