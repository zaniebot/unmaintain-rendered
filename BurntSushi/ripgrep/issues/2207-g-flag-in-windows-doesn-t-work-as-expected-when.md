```yaml
number: 2207
title: "-g flag in Windows doesn't work as expected when following the user guide"
type: issue
state: closed
author: BertanAygun
labels:
  - wontfix
assignees: []
created_at: 2022-05-10T20:49:19Z
updated_at: 2023-11-24T19:59:35Z
url: https://github.com/BurntSushi/ripgrep/issues/2207
synced_at: 2026-01-12T16:13:24Z
```

# -g flag in Windows doesn't work as expected when following the user guide

---

_@BertanAygun_

#### What version of ripgrep are you using?

ripgrep 13.0.0 (rev af6b6c543b)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?
choco
#### What operating system are you using ripgrep on?
Windows

#### Describe your bug.
The user guides show example usage as: 
rg someText -g '*.ext'

But this fails on Windows and --debug shows that .ext files were not being matched.

However the command:
rg someText -g *.ext

works correctly.

Not sure if this is a bug or documentation issue though.


---

_Comment by @BurntSushi on 2022-05-10 21:24_

This is almost certainly a Windows shell thing. You probably need to use double quotes.

---

_Comment by @garoto on 2022-05-10 21:41_

No quotes or double-quotes is the proper way to do it in a `cmd.exe` prompt. The user-guide is written from a *nix shell pov.

---

_Comment by @salsifis on 2022-05-12 07:23_

Quoting with single quotes will work in Powershell or bash, not in CMD.

CMD does not interpret single quotes and will include them in the subprocess command-line.

---

_Comment by @BurntSushi on 2023-11-24 19:59_

I don't think there is anything actionable on my end here.

I've always found cmd.exe quite difficult and very unwieldy to use, especially around quoting. It's unclear why one would use on modern Windows. And in particular, I'm not sure it's worth cluttering up the user guide just to address `cmd.exe`.

---

_Closed by @BurntSushi on 2023-11-24 19:59_

---

_Label `wontfix` added by @BurntSushi on 2023-11-24 19:59_

---
