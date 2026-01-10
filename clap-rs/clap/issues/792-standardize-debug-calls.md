---
number: 792
title: Standardize Debug Calls
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - A-meta
assignees: []
created_at: 2016-12-29T00:08:45Z
updated_at: 2018-08-02T03:29:58Z
url: https://github.com/clap-rs/clap/issues/792
synced_at: 2026-01-10T01:26:35Z
---

# Standardize Debug Calls

---

_Issue opened by @kbknapp on 2016-12-29 00:08_

I'd like to standardize the debug format calls. Probably inline with the format used by `ripgrep` since I've seen this same format used in several places.

Example:


```
DEBUG:<crate>::<fn>: <msg>:<vals>
```

as in

```
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'G',
        'C',
        'C'
    ],
    casei: false
}
```

---

_Label `C: meta` added by @kbknapp on 2016-12-29 00:08_

---

_Label `D: easy` added by @kbknapp on 2016-12-29 00:08_

---

_Label `E: tedious` added by @kbknapp on 2016-12-29 00:08_

---

_Label `P4: nice to have` added by @kbknapp on 2016-12-29 00:08_

---

_Label `T: enhancement` added by @kbknapp on 2016-12-29 00:08_

---

_Label `W: 2.x` added by @kbknapp on 2016-12-29 00:08_

---

_Referenced in [clap-rs/clap#795](../../clap-rs/clap/pulls/795.md) on 2016-12-29 19:07_

---

_Closed by @homu on 2016-12-30 05:30_

---

_Referenced in [Dylan-DPC/clap#1](../../Dylan-DPC/clap/pulls/1.md) on 2022-07-29 07:35_

---
