---
number: 483
title: Colored --help
type: issue
state: closed
author: flying-sheep
labels:
  - C-enhancement
  - A-help
  - E-medium
assignees: []
created_at: 2016-04-12T11:17:50Z
updated_at: 2018-08-02T03:29:49Z
url: https://github.com/clap-rs/clap/issues/483
synced_at: 2026-01-10T01:26:30Z
---

# Colored --help

---

_Issue opened by @flying-sheep on 2016-04-12 11:17_

It would be nice to have an option to autocolor help, e.g. like this:

![](http://i.stack.imgur.com/bYAoq.png)


---

_Comment by @kbknapp on 2016-04-17 22:29_

Hmmm, I really like this idea! I'm just not sure about it being the default option, or leaving up to a cargo feature....thoughts?

Thanks!

Edit: It could also be an [`AppSettings`](http://kbknapp.github.io/clap-rs/clap/enum.AppSettings.html) variant...


---

_Label `T: enhancement` added by @kbknapp on 2016-04-17 22:32_

---

_Label `C: help message` added by @kbknapp on 2016-04-17 22:32_

---

_Label `D: easy` added by @kbknapp on 2016-04-17 22:32_

---

_Label `P3: want to have` added by @kbknapp on 2016-04-17 22:32_

---

_Label `W: 2.x` added by @kbknapp on 2016-04-17 22:32_

---

_Label `M: mentored` added by @kbknapp on 2016-04-17 22:32_

---

_Label `C: settings` added by @kbknapp on 2016-04-17 22:32_

---

_Label `T: new setting` added by @kbknapp on 2016-04-17 22:32_

---

_Comment by @kbknapp on 2016-04-18 04:08_

So I've ended up adding this as a non-default option, but enabled via the `color` cargo feature **and** `AppSettings::ColoredHelp` in #487 

![screenshot](http://i.imgur.com/7fs2h5j.png)


---

_Closed by @homu on 2016-04-18 05:11_

---

_Comment by @flying-sheep on 2016-04-18 08:47_

very cool! thanks!


---

_Comment by @nabijaczleweli on 2016-04-20 19:52_

How about supporting Windooze with this?


---

_Comment by @kbknapp on 2016-04-20 19:55_

@nabijaczleweli that's the plan...I just haven't had time to work on it lately.


---
