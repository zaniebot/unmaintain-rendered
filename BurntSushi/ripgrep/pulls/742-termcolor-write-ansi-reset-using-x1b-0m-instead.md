```yaml
number: 742
title: "termcolor: Write `Ansi::reset()` using `\\x1b[0m` instead of `\\x1b[m`"
type: pull_request
state: merged
author: kennytm
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2018-01-11T20:19:55Z
updated_at: 2018-01-29T20:15:25Z
url: https://github.com/BurntSushi/ripgrep/pull/742
synced_at: 2026-01-12T18:23:13Z
```

# termcolor: Write `Ansi::reset()` using `\x1b[0m` instead of `\x1b[m`

---

_@kennytm_

AppVeyor CI supports coloring the console through ANSI escape codes, however its support is incomplete (appveyor/ci#1824). One particular issue is that `\x1b[m` is not recognized as the same as `\x1b[0m`. 

This caused colored output using this library extremely ugly on AppVeyor as the color is never reset ([example](https://ci.appveyor.com/project/rust-lang/rust/build/1.0.6004/job/a23aftwu88yh35c4), start from line 675).

While technically an AppVeyor bug, I think changing the code here would improve compatibility anyway. (That issue is also untouched for 2 months so I doubt if AppVeyor is going to fix it soon.)



---

_Merged by @BurntSushi on 2018-01-29 19:14_

---

_Closed by @BurntSushi on 2018-01-29 19:14_

---

_Comment by @BurntSushi on 2018-01-29 19:15_

Thanks!

---

_Branch deleted on 2018-01-29 20:15_

---
