---
number: 676
title: "Some tests use env::args() and abort the test suite if -q or --ignored are passed"
type: issue
state: closed
author: tormol
labels:
  - C-bug
assignees: []
created_at: 2016-10-04T18:21:57Z
updated_at: 2018-08-02T03:29:54Z
url: https://github.com/clap-rs/clap/issues/676
synced_at: 2026-01-07T13:12:19-06:00
---

# Some tests use env::args() and abort the test suite if -q or --ignored are passed

---

_Issue opened by @tormol on 2016-10-04 18:21_

- `create_positional()` in tests/positional.rs:
- `create_app()` in tests/tests.rs
- `add_multiple()` in tests/tests.rs

Are they intended to test with `env::args()?

They all accept an empty vec.


---

_Comment by @kbknapp on 2016-10-04 21:15_

Nope, that's a great catch!


---

_Label `T: bug` added by @kbknapp on 2016-10-04 21:15_

---

_Label `P2: need to have` added by @kbknapp on 2016-10-04 21:15_

---

_Label `C: tests` added by @kbknapp on 2016-10-04 21:15_

---

_Label `D: easy` added by @kbknapp on 2016-10-04 21:15_

---

_Label `W: 2.x` added by @kbknapp on 2016-10-04 21:15_

---

_Comment by @kbknapp on 2016-10-04 21:15_

@tormol changing to `get_matches_from(vec![""])` should work


---

_Closed by @kbknapp on 2016-10-04 21:15_

---

_Reopened by @kbknapp on 2016-10-04 21:15_

---

_Comment by @nabijaczleweli on 2016-10-04 21:19_

Confirmed: those are the only uses of `get_matches()`


---

_Referenced in [clap-rs/clap#678](../../clap-rs/clap/pulls/678.md) on 2016-10-04 21:25_

---

_Closed by @homu on 2016-10-04 22:24_

---
