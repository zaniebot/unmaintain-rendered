---
number: 506
title: Run ignored doc tests only on unix systems.
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - A-docs
assignees: []
created_at: 2016-05-13T22:49:07Z
updated_at: 2018-08-02T03:29:49Z
url: https://github.com/clap-rs/clap/issues/506
synced_at: 2026-01-07T13:12:19-06:00
---

# Run ignored doc tests only on unix systems.

---

_Issue opened by @kbknapp on 2016-05-13 22:49_

Some doc tests are only able to run on unix systems, but are currently ignored. I'd like to change that. An example would be the [`Arg::value_of_os`](http://kbknapp.github.io/clap-rs/clap/struct.ArgMatches.html#method.value_of_os) ([src](https://github.com/kbknapp/clap-rs/blob/0c6c4ad743f234774899a9e700d24c794e9aa23e/src/args/arg_matches.rs#L157-L168))


---

_Label `T: enhancement` added by @kbknapp on 2016-05-13 22:49_

---

_Label `C: tests` added by @kbknapp on 2016-05-13 22:49_

---

_Label `C: docs` added by @kbknapp on 2016-05-13 22:49_

---

_Label `D: easy` added by @kbknapp on 2016-05-13 22:49_

---

_Label `P3: want to have` added by @kbknapp on 2016-05-13 22:49_

---

_Label `E: tedious` added by @kbknapp on 2016-05-13 22:49_

---

_Label `W: 2.x` added by @kbknapp on 2016-05-13 22:49_

---

_Referenced in [clap-rs/clap#656](../../clap-rs/clap/pulls/656.md) on 2016-09-10 20:22_

---

_Closed by @homu on 2016-10-16 17:26_

---
