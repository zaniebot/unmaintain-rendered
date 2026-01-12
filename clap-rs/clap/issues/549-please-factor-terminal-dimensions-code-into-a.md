```yaml
number: 549
title: Please factor terminal dimensions code into a crate
type: issue
state: closed
author: joshtriplett
labels:
  - A-meta
assignees: []
created_at: 2016-06-29T23:14:32Z
updated_at: 2018-08-02T03:29:50Z
url: https://github.com/clap-rs/clap/issues/549
synced_at: 2026-01-12T16:14:09Z
```

# Please factor terminal dimensions code into a crate

---

_@joshtriplett_

clap's `src/term.rs` provides functions to detect the terminal size on either Windows or POSIX systems.  I'd love to have a crate providing those functions, for use in applications other than clap.


---

_Comment by @kbknapp on 2016-06-30 01:09_

Sounds like a good idea. Thanks!


---

_Comment by @kbknapp on 2016-06-30 03:22_

it's now in https://crates.io/crates/term_size


---

_Label `C: meta` added by @kbknapp on 2016-06-30 03:28_

---

_Label `T: refacotr` added by @kbknapp on 2016-06-30 03:28_

---

_Label `P4: nice to have` added by @kbknapp on 2016-06-30 03:28_

---

_Label `D: intermediate` added by @kbknapp on 2016-06-30 03:28_

---

_Label `W: 2.x` added by @kbknapp on 2016-06-30 03:28_

---

_Added to milestone `2.8.0` by @kbknapp on 2016-06-30 03:28_

---

_Comment by @joshtriplett on 2016-06-30 04:08_

Thank you!  That was incredibly fast!


---

_Closed by @homu on 2016-06-30 13:44_

---
