---
number: 1002
title: "Provide a `default_values` method taking `&[&str]` to parallel `default_value`"
type: issue
state: closed
author: euclio
labels:
  - C-enhancement
  - E-medium
  - E-easy
assignees: []
created_at: 2017-07-18T16:59:07Z
updated_at: 2021-02-23T18:02:28Z
url: https://github.com/clap-rs/clap/issues/1002
synced_at: 2026-01-07T13:12:19-06:00
---

# Provide a `default_values` method taking `&[&str]` to parallel `default_value`

---

_Issue opened by @euclio on 2017-07-18 16:59_

There should be a way to provide a default list of `&str` to be returned by `values_of` in matches if no argument is supplied.



---

_Comment by @kbknapp on 2017-07-29 21:13_

Good point! This will probably be rolled up into the v3 push. Thanks for suggesting!

---

_Label `T: enhancement` added by @kbknapp on 2018-02-02 01:58_

---

_Label `C: args` added by @kbknapp on 2018-02-02 01:58_

---

_Label `D: easy` added by @kbknapp on 2018-02-02 01:58_

---

_Label `P3: want to have` added by @kbknapp on 2018-02-02 01:58_

---

_Label `W: 3.x` added by @kbknapp on 2018-02-02 01:58_

---

_Label `M: mentored` added by @kbknapp on 2018-07-22 02:32_

---

_Label `good first issue` added by @kbknapp on 2018-07-22 02:32_

---

_Added to milestone `v3.0` by @kbknapp on 2018-07-22 02:32_

---

_Comment by @thiagoarrais on 2019-10-08 16:20_

Is this still valid? I'm interested in offering a PR for it.

---

_Referenced in [clap-rs/clap#1572](../../clap-rs/clap/pulls/1572.md) on 2019-10-10 16:56_

---

_Closed by @Dylan-DPC-zz on 2019-10-22 16:01_

---

_Comment by @LinusCDE on 2021-02-23 18:02_

Would be really cool to have this for derive as well.

---
