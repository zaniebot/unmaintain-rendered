---
number: 4494
title: "Optional value with require_equals leads to debug assertion failure: mismatch between `num_args` (0) and `takes_value`"
type: issue
state: closed
author: dsherret
labels:
  - C-bug
assignees: []
created_at: 2022-11-20T19:17:15Z
updated_at: 2022-11-21T15:21:28Z
url: https://github.com/clap-rs/clap/issues/4494
synced_at: 2026-01-10T01:27:56Z
---

# Optional value with require_equals leads to debug assertion failure: mismatch between `num_args` (0) and `takes_value`

---

_Issue opened by @dsherret on 2022-11-20 19:17_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.65.0

### Clap Version

4.0.26

### Minimal reproducible code

```rust
fn main() {
  let app = clap::Command::new("command").arg(
    clap::Arg::new("incremental")
      .long("incremental")
      .num_args(0..1)
      .value_parser(["true", "false"])
      .action(clap::ArgAction::Set) // if I don't have this it panics
      .require_equals(true),
  );

  app.try_get_matches_from(&["command", "--incremental"]).unwrap();
}
```


### Steps to reproduce the bug with the above code

cargo run

### Actual Behaviour

panicked at 'assertion failed: `(left == right)`
  left: `false`,
 right: `true`: Argument incremental: mismatch between `num_args` (0) and `takes_value`', C:\Users\david\.cargo\registry\src\github.com-1ecc6299db9ec823\clap-4.0.26\src\builder\debug_asserts.rs:769:5

### Expected Behaviour

Should not fail debug assert.

The user may provide no flag, `--incremental`, `--incremental=true`, or `--incremental=false` and I need to know all four states because it leads to different behaviour (well, `--incremental` and `--incremental=true` are the same).

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @dsherret on 2022-11-20 19:17_

---

_Comment by @epage on 2022-11-21 12:51_

The assert is correct, `incremental` does not accept any values.

`.num_args(0..1)` is being set.  In Rust `0..1` is mathematically `[0, 1)`, meaning it includes 0 but excludes 1.  With discrete numbers, this just leaves 0, making it the equivalent of `.num_args(0)`

What the program needs is `.num_args(0..=1)`.  In Rust `0..=1` is mathematically `[0, 1]`, meaning it includes 0 and includes 1.

---

_Closed by @epage on 2022-11-21 12:51_

---

_Comment by @dsherret on 2022-11-21 15:03_

Ugh, right. Thanks!

One feedback is probably the assertion text could be improved. I didn't know what `num_args (0)` meant until you mentioned that.

---

_Comment by @epage on 2022-11-21 15:21_

The problem is that we normalize `num_args` before we get to the assertions, so that was all we knew.

---
