---
number: 3647
title: "arg!: parsing problem of whether Arg or Value is required"
type: issue
state: closed
author: Yogaflre
labels:
  - C-bug
assignees: []
created_at: 2022-04-22T07:20:19Z
updated_at: 2022-04-22T12:15:58Z
url: https://github.com/clap-rs/clap/issues/3647
synced_at: 2026-01-10T01:27:44Z
---

# arg!: parsing problem of whether Arg or Value is required

---

_Issue opened by @Yogaflre on 2022-04-22 07:20_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.60.0

### Clap Version

3.1.9

### Minimal reproducible code

```rust
fn main() {
    let command = Command::new("test")
        .arg(arg!(--foo [FOO]))
        .arg(arg!(--bar <BAR>))
        .group(ArgGroup::new("group").args(&["foo", "bar"]).required(true));
    // assert!(command.try_get_matches_from(["", "--bar", "value"]).is_ok());
    assert!(command.try_get_matches_from(["", "--foo", "value"]).is_ok());
}
```


### Steps to reproduce the bug with the above code

cargo run

### Actual Behaviour

"test --bar value" succeeded.
"test --foo value" failed.(Errorkind: MissingRequiredArgument) Because \<BAR\> will set --bar as REQUIRED.

### Expected Behaviour

test <--foo [\<FOO\>...]|--bar \<BAR\>>

Group args should be mutually exclusive. 
The setting for REQUIRED in arg! marco should be more explicit.

### Additional Context

Question: #3641

### Debug Output

_No response_

---

_Label `C-bug` added by @Yogaflre on 2022-04-22 07:20_

---

_Referenced in [clap-rs/clap#3650](../../clap-rs/clap/pulls/3650.md) on 2022-04-22 12:03_

---

_Comment by @epage on 2022-04-22 12:04_

Just happened to fix this yesterday as part of some refactors though didn't end up adding a test for it, so closing this out when the test gets merged.

---

_Closed by @epage on 2022-04-22 12:15_

---
