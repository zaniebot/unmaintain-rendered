```yaml
number: 5079
title: "`UnknownArgumentValueParser` eagerly errors out"
type: issue
state: closed
author: weihanglo
labels:
  - C-bug
assignees: []
created_at: 2023-08-18T11:22:07Z
updated_at: 2023-08-18T21:06:34Z
url: https://github.com/clap-rs/clap/issues/5079
synced_at: 2026-01-12T16:14:16Z
```

# `UnknownArgumentValueParser` eagerly errors out

---

_@weihanglo_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.72.0-beta.8 (598a0a3cb 2023-08-12)

### Clap Version

4.3.22

### Minimal reproducible code

```rust
#!/usr/bin/env -S cargo +nightly -Zscript

//! ```cargo
//! [dependencies]
//! clap = "=4.3.22"
//! ```

fn main() {
    let _cmd = clap::Command::new("test")
        .args([
            clap::Arg::new("ignore-rust-version")
                .long("ignore-rust-version")
                .action(clap::ArgAction::SetTrue),
            clap::Arg::new("libtest-ignore")
                .long("ignored")
                .action(clap::ArgAction::SetTrue)
                .value_parser(
                    clap::builder::UnknownArgumentValueParser::suggest_arg("-- --ignored")
                        .and_suggest("not much else to say"),
                )
                .hide(true),
        ])
        .get_matches();
}
```

### Steps to reproduce the bug with the above code

See above cargo script.

### Actual Behaviour

```console
$ cargo +nightly -Zscript clap-unknown-suggest-bug.rs

error: unexpected argument '--ignored' found

  tip: a similar argument exists: '-- --ignored'
  tip: not much else to say

Usage: clap-unknown-suggest-bug [OPTIONS]

For more information, try '--help'.
```

### Expected Behaviour

Should not fail

### Additional Context

cc https://github.com/clap-rs/clap/pull/5075

Sorry I wasn't familiar with codebase enough to detect this bug during the review 😞.

### Debug Output

_No response_

---

_Label `C-bug` added by @weihanglo on 2023-08-18 11:22_

---

_Referenced in [clap-rs/clap#5080](../../clap-rs/clap/pulls/5080.md) on 2023-08-18 19:30_

---

_Comment by @epage on 2023-08-18 19:30_

I had thought of this case but forgot it.  The issue is we model a flag being present as a `default_missing_value` and not being present with `default_value`.  In either case, we end up parsing a value.

A fix and new release will be available shortly

---

_Comment by @weihanglo on 2023-08-18 19:33_

Thanks a lot!

BTW, I feel the power of cargo-script 💪🏾. 

---

_Closed by @epage on 2023-08-18 21:06_

---
