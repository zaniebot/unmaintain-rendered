```yaml
number: 371
title: High CPU usage in 0.3.2, low in 0.4.0
type: issue
state: closed
author: stej
labels: []
assignees: []
created_at: 2017-02-20T17:37:58Z
updated_at: 2017-02-21T12:50:44Z
url: https://github.com/BurntSushi/ripgrep/issues/371
synced_at: 2026-01-12T16:13:21Z
```

# High CPU usage in 0.3.2, low in 0.4.0

---

_@stej_

I downloaded two versions - ripgrep-0.3.2-x86_64-pc-windows-msvc.zip and ripgrep-0.4.0-x86_64-pc-windows-msvc.zip extracted and ran ripgrep with this command: `rg -F 32595217 -g 2017-01*`. 

I noticed that when using the 0.3.2 version, CPU usage is quite high (which could be understandable), but very low for 0.4.0 version. I would expect that the very low CPU for 0.4.0 is quite suspicious. Can you explain it please?

![2017-02-20_17h40_17](https://cloud.githubusercontent.com/assets/253560/23136129/891d4a16-f79b-11e6-84e6-f2148d650991.png)

(sshot is from ProcessExplorer where top window is OS statistics, bottom window is for ripgrep only)

---

_Comment by @BurntSushi on 2017-02-20 18:54_

This might be a result of fixing #258.

> Can you explain it please?

I would if I could. But there's not enough information here. I'd need to get a fully reproducible example, preferably on Linux, because that's where I'm comfortable doing performance analysis.

---

_Comment by @stej on 2017-02-20 21:31_

Sorry, no Linux repro. Windows only. 
I'm curious, because I guess that if I create an app that uses several threads to read from files and looks for some text in it (e.g. some sort of IndexOf), it would certainly take much more CPU than ripgrep 0.4.0. Also when regexes would be used, I would expect CPU to rise even more. 

---

_Comment by @BurntSushi on 2017-02-20 21:37_

@stej There are too many assumptions in your expectations unfortunately. That's why I need a repro. For example, if your search is hitting disk, then search will be bottlenecked by IO, which will keep CPU usage quite low. It would be better to compare the results. Is there any difference between `0.3.2` and `0.4`?

---

_Comment by @BurntSushi on 2017-02-21 12:50_

I'm going to close this. The fix to #258 happened between `0.3.2` and `0.4.0`, and was specifically fixing a case where the CPU could burn. In the issue, I described some of the cases in which it could happen, but those are merely the *likely* cases. In fact, it could happen in any search where one thread was getting all of the work while the rest were starved. Please see the commit message in 95cea7 for more details: https://github.com/BurntSushi/ripgrep/commit/95cea77625f978a21b1d923e54d13f9217f0ef30

---

_Closed by @BurntSushi on 2017-02-21 12:50_

---
