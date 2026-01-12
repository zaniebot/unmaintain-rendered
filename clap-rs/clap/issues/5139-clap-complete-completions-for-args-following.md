```yaml
number: 5139
title: "clap_complete: completions for args following subcommand are missing"
type: issue
state: closed
author: scootermon
labels:
  - C-bug
assignees: []
created_at: 2023-09-25T21:01:37Z
updated_at: 2023-09-25T21:32:36Z
url: https://github.com/clap-rs/clap/issues/5139
synced_at: 2026-01-12T16:14:16Z
```

# clap_complete: completions for args following subcommand are missing

---

_@scootermon_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.72.1 (d5c2e9c34 2023-09-13)

### Clap Version

clap: 4.4.4, clap_complete: 4.4.1

### Minimal reproducible code

```rust
fn main() {
    #[derive(Clone, clap::ValueEnum)]
    enum Enum {
        A,
        B,
        C,
    }

    // create a dummy command with a subcommand, which in turn takes an enum arg
    let mut app = clap::Command::new("mre").subcommand(
        clap::Command::new("subcmd").arg(
            clap::Arg::new("arg")
                .required(true)
                .value_parser(clap::builder::EnumValueParser::<Enum>::new()),
        ),
    );

    // ask clap_complete to generate completions for the argument following the subcommand
    let completions = clap_complete::dynamic::complete(
        &mut app,
        vec!["mre".into(), "subcmd".into(), "".into()],
        2,
        None,
    )
    .unwrap();
    // this will panic because 'A', 'B', 'C' are missing (only '-h' and '--help' are listed)
    assert!(completions.len() >= 3);
}
```


### Steps to reproduce the bug with the above code

`cargo run` is enough as long as the `"unstable-dynamic"` is active for `clap_complete`.

### Actual Behaviour

clap_complete doesn't suggest any values for arguments following a subcommand.

### Expected Behaviour

It should suggest possible values for arguments following a subcommand.

### Additional Context

The reason for this seems fairly straight-forward:

The code responsible for adding the arg value completions uses the `pos_index` passed into the function to find the correct `clap::Arg`:

https://github.com/clap-rs/clap/blob/0d9b14fa6e9ccb09fea5d7c93477b7fca8d51bdc/clap_complete/src/dynamic/completer.rs#L150-L155

This fails because the `pos_index` in our example is set to 0. The `pos_index` is always set to 0 when clap_complete encounters a subcommand:

https://github.com/clap-rs/clap/blob/0d9b14fa6e9ccb09fea5d7c93477b7fca8d51bdc/clap_complete/src/dynamic/completer.rs#L64-L68


I fail to see why this is done. If the value is set to 1 instead, the example starts working.

### Debug Output

_No response_

---

_Label `C-bug` added by @scootermon on 2023-09-25 21:01_

---

_Referenced in [clap-rs/clap#5140](../../clap-rs/clap/pulls/5140.md) on 2023-09-25 21:15_

---

_Closed by @epage on 2023-09-25 21:31_

---

_Comment by @epage on 2023-09-25 21:32_

Good to know people are trying out the Rust-implemeted completions.  clap_complete-v4.4.2 is now out with the fix.

---
