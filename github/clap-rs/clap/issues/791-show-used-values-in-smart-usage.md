---
number: 791
title: "Show used values in \"smart usage\""
type: issue
state: open
author: kbknapp
labels:
  - C-enhancement
  - A-help
  - E-hard
assignees: []
created_at: 2016-12-28T22:59:50Z
updated_at: 2021-12-09T16:59:27Z
url: https://github.com/clap-rs/clap/issues/791
synced_at: 2026-01-07T13:12:19-06:00
---

# Show used values in "smart usage"

---

_Issue opened by @kbknapp on 2016-12-28 22:59_

It would be nice to show actual values in the context sensitive usage statements:

i.e. current `clap`

```
$ prog --option val other.conf -a
error: -a isn't valid

usage:
    prog --option <some> <file>
```

What I'd like to see


```
$ prog --option val other.conf -a
error: -a isn't valid

usage:
    prog --option val other.conf
```

---

_Label `C: errors` added by @kbknapp on 2016-12-28 22:59_

---

_Label `D: intermediate` added by @kbknapp on 2016-12-28 22:59_

---

_Label `P4: nice to have` added by @kbknapp on 2016-12-28 22:59_

---

_Label `T: enhancement` added by @kbknapp on 2016-12-28 22:59_

---

_Label `W: 2.x` added by @kbknapp on 2016-12-28 22:59_

---

_Added to milestone `2.20.0` by @kbknapp on 2016-12-28 22:59_

---

_Removed from milestone `2.20.0` by @kbknapp on 2017-01-04 04:04_

---

_Comment by @kbknapp on 2017-01-04 04:05_

I ran into some snags while implementing this, values should be in the same order as they were originally placed in, but this requires some additional work. I'll have to play with it some more.

---

_Label `D: hard` added by @kbknapp on 2017-01-04 04:05_

---

_Label `D: intermediate` removed by @kbknapp on 2017-01-04 04:05_

---

_Label `W: 3.x` added by @kbknapp on 2018-07-22 01:05_

---

_Label `W: 2.x` removed by @kbknapp on 2018-07-22 01:05_

---

_Added to milestone `3.0.0-alpha.3` by @pksunkara on 2020-02-05 08:38_

---

_Removed from milestone `3.0.0-alpha.3` by @pksunkara on 2020-02-05 08:38_

---

_Added to milestone `3.1` by @pksunkara on 2020-02-05 08:38_

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Referenced in [epage/clapng#67](../../epage/clapng/issues/67.md) on 2021-12-06 16:31_

---

_Label `C: errors` removed by @epage on 2021-12-08 20:26_

---

_Label `A-help` added by @epage on 2021-12-08 20:26_

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 16:59_

---

_Removed from milestone `3.1` by @epage on 2021-12-09 16:59_

---

_Referenced in [clap-rs/clap#5748](../../clap-rs/clap/issues/5748.md) on 2024-09-24 21:40_

---
