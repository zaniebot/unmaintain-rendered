---
number: 518
title: "usage: Put positional args direclty in usage string"
type: issue
state: closed
author: kamalmarhubi
labels:
  - C-enhancement
  - E-medium
assignees: []
created_at: 2016-06-01T11:25:03Z
updated_at: 2020-04-12T10:09:27Z
url: https://github.com/clap-rs/clap/issues/518
synced_at: 2026-01-07T13:12:19-06:00
---

# usage: Put positional args direclty in usage string

---

_Issue opened by @kamalmarhubi on 2016-06-01 11:25_

If I have just one optional positional arg say `FILE`, it should show up directly in the usage string, instead of as `[ARGS]`. If it's required it shows up as `<FILE>`, but if it's optional I get `[ARGS]` with it listed under the `ARGS` section in help. I'd prefer `[FILE]` directly in usage.


---

_Comment by @kbknapp on 2016-06-01 23:18_

I hadn't thought about that before, I like that. My only concern would be handling conflicts, or if users get confused why adding more positional args makes them disappear from the default usage string.

Thoughts?


---

_Label `T: enhancement` added by @kbknapp on 2016-06-01 23:19_

---

_Label `P4: nice to have` added by @kbknapp on 2016-06-01 23:19_

---

_Label `D: easy` added by @kbknapp on 2016-06-01 23:19_

---

_Label `W: 2.x` added by @kbknapp on 2016-06-01 23:19_

---

_Label `M: mentored` added by @kbknapp on 2016-06-01 23:19_

---

_Label `C: usage strings` added by @kbknapp on 2016-06-01 23:19_

---

_Comment by @kamalmarhubi on 2016-06-02 09:28_

> My only concern would be handling conflicts

As in different args with the same `value_name`?

> if users get confused why adding more positional args makes them disappear from the default usage string.

Perhaps it could be an app or arg setting then?


---

_Comment by @kbknapp on 2016-06-02 10:23_

> As in different args with the same value_name?

No, I mean if the positional arg conflicts with specific options or flags. But on the flip side, the user would a message with new usage string so I guess it's not that big a deal.

> Perhaps it could be an app or arg setting then?

I thought about that, but I think its benign enough to be the default and add a setting to disable this feature.


---

_Comment by @kbknapp on 2016-06-04 15:58_

#520 fixes this


---

_Closed by @homu on 2016-06-08 01:34_

---

_Added to milestone `2.6.0` by @kbknapp on 2016-06-08 02:06_

---

_Label `C: usage strings` added by @pksunkara on 2020-04-12 10:09_

---

_Label `C: usage string parser` removed by @pksunkara on 2020-04-12 10:09_

---
