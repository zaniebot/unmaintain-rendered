---
number: 3648
title: Breaking change in the 3.1.11 release
type: issue
state: closed
author: weiznich
labels:
  - C-bug
assignees: []
created_at: 2022-04-22T08:09:08Z
updated_at: 2022-04-22T11:52:50Z
url: https://github.com/clap-rs/clap/issues/3648
synced_at: 2026-01-07T13:12:19-06:00
---

# Breaking change in the 3.1.11 release

---

_Issue opened by @weiznich on 2022-04-22 08:09_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.54.0 (a178d0322 2021-07-26)

### Clap Version

3.1.11

### Minimal reproducible code

Sorry, cannot provide that right now. I've observing failing tests due to a clap update in diesel_cli. See [here](https://github.com/diesel-rs/diesel/runs/6124975155?check_suite_focus=true) for a failing test run.

### Steps to reproduce the bug with the above code

```
git clone https://github.com/diesel-rs/diesel
cd diesel/diesel_cli
DATABASE_URL=/tmp/test.db cargo test --features "sqlite sqlite" --no-default-features # watch the tests failing
cargo update -p clap:3.1.11 --precise=3.1.10
DATABASE_URL=/tmp/test.db cargo test --features "sqlite sqlite" --no-default-features # all tests are passing now
```

### Actual Behaviour

Test fail

### Expected Behaviour

Test should not fail

### Additional Context

I will update the this issue with more details as soon as I found the actual reason which part of the diesel cli interface is affected by this change.

### Debug Output

_No response_

---

_Label `C-bug` added by @weiznich on 2022-04-22 08:09_

---

_Comment by @weiznich on 2022-04-22 08:22_

This was introduced by https://github.com/clap-rs/clap/commit/6a9a5d05b0bb3bf96ea5aafdf001f2202ea07e8d. Diesel tests pass before that commit, but fail afterwards.

---

_Referenced in [diesel-rs/diesel#3093](../../diesel-rs/diesel/pulls/3093.md) on 2022-04-22 08:43_

---

_Referenced in [diesel-rs/diesel#3134](../../diesel-rs/diesel/pulls/3134.md) on 2022-04-22 08:44_

---

_Comment by @weiznich on 2022-04-22 11:52_

Closed by #3646 

---

_Closed by @weiznich on 2022-04-22 11:52_

---

_Comment by @epage on 2022-04-22 11:52_

v3.1.12 s released (I was worried about this very case and triple checked for it!)

---

_Referenced in [clap-rs/clap#3652](../../clap-rs/clap/pulls/3652.md) on 2022-04-22 15:47_

---
