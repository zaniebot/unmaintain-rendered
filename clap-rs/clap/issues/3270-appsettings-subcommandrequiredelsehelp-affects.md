```yaml
number: 3270
title: "AppSettings::SubcommandRequiredElseHelp affects \"leaf\" subcommands that don't have own subcommands"
type: issue
state: closed
author: heroin-moose
labels:
  - C-bug
  - A-builder
  - S-duplicate
assignees: []
created_at: 2022-01-08T18:17:40Z
updated_at: 2022-01-11T18:21:21Z
url: https://github.com/clap-rs/clap/issues/3270
synced_at: 2026-01-12T16:14:14Z
```

# AppSettings::SubcommandRequiredElseHelp affects "leaf" subcommands that don't have own subcommands

---

_@heroin-moose_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.57.0 (f1edd0429 2021-11-29)

### Clap Version

3.0.5

### Minimal reproducible code

```rust
use clap::App;
use clap::AppSettings;

fn main() {
    let args = App::new("test")
        .global_setting(AppSettings::InferSubcommands)
        .global_setting(AppSettings::SubcommandRequiredElseHelp)
        .subcommands(vec![
            App::new("parent")
                .subcommands(vec![
                    App::new("child")
                ])
        ]);

    args.get_matches();
}
```


### Steps to reproduce the bug with the above code

cargo run -- parent child

### Actual Behaviour

A help message is displayed and program exists with status 2.

### Expected Behaviour

This code should not trigger an error because commands that don't have subcommands should not be affected by this flag.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @heroin-moose on 2022-01-08 18:17_

---

_Label `A-builder` added by @epage on 2022-01-10 14:45_

---

_Label `S-triage` added by @epage on 2022-01-10 14:45_

---

_Comment by @epage on 2022-01-10 14:57_

Looks like this is a duplicate of https://github.com/clap-rs/clap/issues/2513 (which uses yaml, making searching for it harder) .  Let's continue the conversation over there.

---

_Closed by @epage on 2022-01-10 14:57_

---

_Referenced in [clap-rs/clap#2513](../../clap-rs/clap/issues/2513.md) on 2022-01-10 15:43_

---

_Label `S-triage` removed by @epage on 2022-01-11 18:21_

---

_Label `S-duplicate` added by @epage on 2022-01-11 18:21_

---
