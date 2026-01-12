```yaml
number: 661
title: Ctrl+C handling in Windows can bleed coloring
type: issue
state: closed
author: calonx
labels:
  - wontfix
assignees: []
created_at: 2017-11-01T15:17:53Z
updated_at: 2017-11-01T15:36:42Z
url: https://github.com/BurntSushi/ripgrep/issues/661
synced_at: 2026-01-12T16:13:22Z
```

# Ctrl+C handling in Windows can bleed coloring

---

_@calonx_

I accidentally made my search a bit too loose and found tons of useless results scrolling by on my screen. As works with many other command-line utilities, I used Ctrl+C to abort the search -- but I got unlucky and happened to abort while in a colored sequence, so the color then bled into my whole prompt session.

![image](https://user-images.githubusercontent.com/1431773/32281594-a5acba1e-bef5-11e7-906a-32a6f7652cee.png)

(the bottom two lines should not contain red text; should be the standard off-white for cmd.exe)

---

_Comment by @calonx on 2017-11-01 15:19_

...and I just now found this closed issue:

https://github.com/BurntSushi/ripgrep/issues/430

Does ripgrep currently do any special handling of Ctrl+C on Windows?

Edit: A quick ripgrep of ripgrep shows no calls to SetConsoleCtrlHandler, which suggests that the answer is "no". It's not hard to set up such a thing:

https://docs.microsoft.com/en-us/windows/console/registering-a-control-handler-function

Edit 2: Looks like I made the silly assumption that this was a C/C++ utility! Anyway, it seems that other people have found ways to register Ctrl+C handlers:

https://github.com/Detegr/rust-ctrlc

---

_Comment by @BurntSushi on 2017-11-01 15:32_

This is a known issue. It's described a bit in a tangent here: https://github.com/BurntSushi/ripgrep/issues/281 TL;DR - "Add `^C` handling" is not a complete answer.

As of now, this is not something I plan on fixing.

---

_Closed by @BurntSushi on 2017-11-01 15:32_

---

_Label `wontfix` added by @BurntSushi on 2017-11-01 15:32_

---

_Comment by @calonx on 2017-11-01 15:36_

Gotcha, I'll read that over -- thanks!

---
