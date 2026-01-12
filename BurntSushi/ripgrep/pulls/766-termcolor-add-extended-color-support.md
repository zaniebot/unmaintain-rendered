```yaml
number: 766
title: "termcolor: add extended color support"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/extended-colors
created_at: 2018-01-29T23:26:24Z
updated_at: 2018-01-29T23:49:56Z
url: https://github.com/BurntSushi/ripgrep/pull/766
synced_at: 2026-01-12T18:23:13Z
```

# termcolor: add extended color support

---

_@BurntSushi_

This commit adds 256-color and 24-bit truecolor support to ripgrep.
This only provides output support on ANSI terminals. If the Windows
console is used for coloring, then 256-color and 24-bit color settings
are ignored.

This PR is based on #452. In the interest of getting it merged, I did a bit of cleanup.  Namely, on Windows, unsupported colors are just ignored instead of falling back to white. I also improved the error reporting when attempting to parse RGB/256-color settings.

I did manual QA on Windows and Linux and everything seems to be working as I'd expect! Thanks @tiehuis!

---

_Merged by @BurntSushi on 2018-01-29 23:49_

---

_Closed by @BurntSushi on 2018-01-29 23:49_

---

_Branch deleted on 2018-01-29 23:49_

---
