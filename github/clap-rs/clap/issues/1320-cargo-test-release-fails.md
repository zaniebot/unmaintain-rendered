---
number: 1320
title: cargo test --release fails
type: issue
state: closed
author: matthiaskrgr
labels: []
assignees: []
created_at: 2018-07-18T13:42:41Z
updated_at: 2018-08-02T03:30:26Z
url: https://github.com/clap-rs/clap/issues/1320
synced_at: 2026-01-07T13:12:19-06:00
---

# cargo test --release fails

---

_Issue opened by @matthiaskrgr on 2018-07-18 13:42_

Found this while trying to gather info on another ticket.

repo is @ 570b4e1a77f033dd99de587b6883afb0bb104399

````cargo test```` passes, but in release mode
````cargo test --release```` it does not:

````

failures:
failures:
    low_index_positional_last_multiple_too
    low_index_positional_not_required
    low_index_positional_too_far_back
test result: FAILED. 48 passed; 3 failed; 0 ignored; 0 measured; 0 filtered out
error: test failed, to rerun pass '--test multiple_values'
````

rustc 1.29.0-nightly (4f3c7a472 2018-07-17)


---

_Referenced in [clap-rs/clap#1321](../../clap-rs/clap/issues/1321.md) on 2018-07-18 13:54_

---

_Comment by @kbknapp on 2018-07-21 02:43_

Interesting, thanks for reporting! I'm curious how often this occurs that tests in crates pass on debug but not release...as I've never thought to test both cases. I'll play with this and see if I can get to the root of it.

---

_Label `P2: need to have` added by @kbknapp on 2018-07-21 02:43_

---

_Label `C: tests` added by @kbknapp on 2018-07-21 02:43_

---

_Comment by @kbknapp on 2018-07-21 14:56_

Ah, ok I found the cause. It's because the tests that are failing on `--release` are marked with `#[should_panic]` and those panics only happen in debug builds (i.e. `debug_assertions`).

I'm going to close this as by-design. :smile: 

---

_Closed by @kbknapp on 2018-07-21 14:56_

---
