---
number: 355
title: "Return &[&str] from ArgMatches::values_of"
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - E-medium
assignees: []
created_at: 2015-11-30T12:08:47Z
updated_at: 2018-08-02T03:29:47Z
url: https://github.com/clap-rs/clap/issues/355
synced_at: 2026-01-07T13:12:19-06:00
---

# Return &[&str] from ArgMatches::values_of

---

_Issue opened by @kbknapp on 2015-11-30 12:08_

Currently a `Vec<&str>` is returned, but it would be far more idiomatic and generic to return a slice.

Caveat, this **must** be done in a backwards compat way. If it can't be done in a backwards compat way it'll have to wait for 2.x


---

_Label `T: enhancement` added by @kbknapp on 2015-11-30 12:08_

---

_Label `D: easy` added by @kbknapp on 2015-11-30 12:08_

---

_Label `P3: want to have` added by @kbknapp on 2015-11-30 12:08_

---

_Label `C: matches` added by @kbknapp on 2015-11-30 12:08_

---

_Label `W: 1.x` added by @kbknapp on 2015-11-30 12:08_

---

_Label `M: mentored` added by @kbknapp on 2015-11-30 12:08_

---

_Comment by @kbknapp on 2015-12-08 13:09_

After looking into this, we can return an iterator, but not just a slice as the back end is a `VecMap` and not just a `Vec`.


---

_Added to milestone `1.6.0` by @kbknapp on 2015-12-18 08:57_

---

_Added to milestone `v2.0` by @kbknapp on 2016-01-23 15:49_

---

_Removed from milestone `1.6.0` by @kbknapp on 2016-01-23 15:49_

---

_Comment by @kbknapp on 2016-01-25 09:44_

This is closed with 826bc50

And the new code returns a `Values` struct which implements `Iterator` and `DoubleEndedIterator` and essentially gets exactly what I wanted (i.e. removing the need to allocate a `Vec` to iterate through the values, and being locked in to that one collection type.)


---

_Label `W: 2.x` added by @kbknapp on 2016-01-25 09:44_

---

_Label `W: 1.x` removed by @kbknapp on 2016-01-25 09:44_

---

_Closed by @kbknapp on 2016-01-25 09:45_

---
