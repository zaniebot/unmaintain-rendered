---
number: 427
title: Add support for next line help
type: issue
state: closed
author: kbknapp
labels:
  - A-help
  - A-builder
assignees: []
created_at: 2016-02-17T11:36:34Z
updated_at: 2018-08-02T03:29:48Z
url: https://github.com/clap-rs/clap/issues/427
synced_at: 2026-01-07T13:12:19-06:00
---

# Add support for next line help

---

_Issue opened by @kbknapp on 2016-02-17 11:36_

Add support, either by arg, or as a program whole for displaying the help string on the next line indented by one tab.

```
    -a, --some-long-option <with> <values>    Some really long explanation about how to use this argument and other documenation
```

Would be come

```
    -a, --some-long-option <with> <values>
         Some really long explanation about how to use this argument and other documenation
```


---

_Label `C: args` added by @kbknapp on 2016-02-17 11:39_

---

_Label `C: help message` added by @kbknapp on 2016-02-17 11:39_

---

_Label `C: app` added by @kbknapp on 2016-02-17 11:39_

---

_Label `D: easy` added by @kbknapp on 2016-02-17 11:39_

---

_Label `P3: want to have` added by @kbknapp on 2016-02-17 11:39_

---

_Label `W: 2.x` added by @kbknapp on 2016-02-17 11:39_

---

_Label `C: settings` added by @kbknapp on 2016-02-17 11:39_

---

_Label `T: new setting` added by @kbknapp on 2016-02-17 11:39_

---

_Referenced in [clap-rs/clap#425](../../clap-rs/clap/pulls/425.md) on 2016-02-18 08:06_

---

_Closed by @homu on 2016-02-19 13:53_

---
