```yaml
number: 420
title: How to install ripgrep in atom editor
type: issue
state: closed
author: Er-Kalpesh
labels:
  - question
assignees: []
created_at: 2017-03-27T03:55:21Z
updated_at: 2017-03-28T19:50:30Z
url: https://github.com/BurntSushi/ripgrep/issues/420
synced_at: 2026-01-12T16:13:22Z
```

# How to install ripgrep in atom editor

---

_@Er-Kalpesh_

I tried to install package but it shows the error like : 

**Failed to load burntSushi/ripgrep - try again later.**

but above package is required as mention here : https://github.com/faceair/atom-goto-definition

Please help me.

---

_Comment by @BurntSushi on 2017-03-27 11:33_

The installation instructions are here: https://github.com/BurntSushi/ripgrep#installation

I don't understand the error message you've given and I don't know anything about Atom or what it has to do with how your search tool is installed. I'd recommend filing an issue on the `atom-goto-definition` repo.

---

_Label `question` added by @BurntSushi on 2017-03-27 11:33_

---

_Comment by @halumein on 2017-03-28 17:53_

Same question. Still can't get how to install it on windows. 

---

_Comment by @BurntSushi on 2017-03-28 18:00_

Folks, I can't help you if you don't help me understand your problems. Please tell me in as much detail as possible what you did, what you saw and what you expected to see. Did you run commands? If so, which ones and what were their output? Did you download executables and run them? If so, what happened? Did you see any errors? What version of Windows do you have?

"It doesn't work" **is not helpful**. Please provide more details.

---

_Comment by @halumein on 2017-03-28 18:25_

I'll try to describe.
I've download binary ripgrep-0.5.0-i686-pc-windows-gnu.zip. Unzip it. Now i have thu rg.exe file.
If i run in commandline, it will work. So...
Do i need some more actions, or this is final state of "install" package? (download and unzip).
May be i need make something with PATH-variables, so system can see this file?

![d014d68b91](https://cloud.githubusercontent.com/assets/5361342/24420917/f565dfba-140d-11e7-9c7f-75f6a3c1aa85.jpg)


---

_Comment by @BurntSushi on 2017-03-28 19:50_

> Do i need some more actions, or this is final state of "install" package?

As far as ripgrep is concerned, that's it.

> May be i need make something with PATH-variables, so system can see this file?

Yes. If another tool simply calls `rg.exe` and the directory containing `rg.exe` is not in your `PATH`, then the system does not know how to find `rg.exe`. Therefore, you must add the directory containing `rg.exe` to your `PATH`.

---

_Closed by @BurntSushi on 2017-03-28 19:50_

---
