```yaml
number: 347
title: "document that keyboard interrupt doesn't restore the termcolor on Windows"
type: issue
state: closed
author: jmcomets
labels:
  - doc
assignees: []
created_at: 2017-02-03T12:07:09Z
updated_at: 2017-03-08T15:18:21Z
url: https://github.com/BurntSushi/ripgrep/issues/347
synced_at: 2026-01-12T16:13:21Z
```

# document that keyboard interrupt doesn't restore the termcolor on Windows

---

_@jmcomets_

Hi everyone,

First of all, great job on this. :+1: 

Now on to my issue: I'm using stable Rust (stable-x86_64-pc-windows-msvc + rustc 1.14.0) and installed ripgrep using `cargo install`.
When running a basic `rg --type cpp <pattern>` and pressing Ctrl-C in the middle, cmd's foreground stays red/purple/whatever color was last printed. I haven't tried on Linux to see if this is really platform specific.

---

_Comment by @BurntSushi on 2017-02-03 12:10_

PR #187 actually added this functionality.

I later removed it to fix #281. My reasoning is here: https://github.com/BurntSushi/ripgrep/issues/281#issuecomment-269093893

TL;DR - There are significant trade offs for doing `^C` handling, and IMO, screwing up the colors in the terminal (which is easy to fix by running `echo -ne "\033[0m"` or `color` on Windows I believe) is the least worst choice.

---

_Label `wontfix` added by @BurntSushi on 2017-02-03 12:10_

---

_Closed by @BurntSushi on 2017-02-03 12:10_

---

_Comment by @jmcomets on 2017-02-03 13:59_

I 100% understand the decision. Would you be open to adding this issue to a "faq" or "issues" section in the readme? 

---

_Reopened by @BurntSushi on 2017-02-03 14:06_

---

_Comment by @BurntSushi on 2017-02-03 14:06_

@jmcomets Great idea.

---

_Label `doc` added by @BurntSushi on 2017-02-03 14:06_

---

_Label `wontfix` removed by @BurntSushi on 2017-02-03 14:06_

---

_Renamed from "Keyboard interrupt doesn't restore the termcolor on Windows" to "document that keyboard interrupt doesn't restore the termcolor on Windows" by @BurntSushi on 2017-02-03 14:07_

---

_Closed by @BurntSushi on 2017-03-08 15:18_

---
