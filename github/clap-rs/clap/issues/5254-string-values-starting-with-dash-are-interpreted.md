---
number: 5254
title: string values starting with dash are interpreted as flags
type: issue
state: closed
author: andrewbanchich
labels:
  - C-bug
assignees: []
created_at: 2023-12-09T17:40:29Z
updated_at: 2023-12-13T11:29:36Z
url: https://github.com/clap-rs/clap/issues/5254
synced_at: 2026-01-07T13:12:20-06:00
---

# string values starting with dash are interpreted as flags

---

_Issue opened by @andrewbanchich on 2023-12-09 17:40_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.74.1

### Clap Version

4.3.8

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser)]
struct Cli {
    #[clap(long)]
    foo: String,
}

fn main() {
    let _cli = Cli::parse();
}

```


### Steps to reproduce the bug with the above code

`cargo run -- --foo '-bar'

### Actual Behaviour

Above command produces:

```
Running `target/debug/foo --foo -bar`
error: unexpected argument '-b' found

Usage: foo --foo <FOO>

For more information, try '--help'.
```

Other programs handle `-` correctly (e.g. `echo '-bar'` prints `-bar`)

### Expected Behaviour

Clap interprets anything inside a string as a string.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @andrewbanchich on 2023-12-09 17:40_

---

_Referenced in [andrewbanchich/shreddit#93](../../andrewbanchich/shreddit/issues/93.md) on 2023-12-09 17:41_

---

_Comment by @epage on 2023-12-11 16:55_

This is a feature intended to catch bugs as most of the time an argument won't start with a `-`.

You can override this with [allow_hyphen_values](https://docs.rs/clap/latest/clap/struct.Arg.html#method.allow_hyphen_values) (note: all builder methods also work as derive attributes)

---

_Comment by @andrewbanchich on 2023-12-13 11:29_

Gotcha, thanks!

---

_Closed by @andrewbanchich on 2023-12-13 11:29_

---

_Referenced in [thin-edge/thin-edge.io#3618](../../thin-edge/thin-edge.io/issues/3618.md) on 2025-05-14 06:59_

---
