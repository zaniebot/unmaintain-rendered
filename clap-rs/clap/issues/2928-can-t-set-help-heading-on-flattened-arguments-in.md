---
number: 2928
title: "Can't set help_heading on flattened arguments in `clap_derive`"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - A-derive
assignees: []
created_at: 2021-10-23T16:09:21Z
updated_at: 2021-10-25T00:40:22Z
url: https://github.com/clap-rs/clap/issues/2928
synced_at: 2026-01-10T01:27:29Z
---

# Can't set help_heading on flattened arguments in `clap_derive`

---

_Issue opened by @epage on 2021-10-23 16:09_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

master

### Minimal reproducible code

```rust
#[test]
fn flattener_set_help_heading() {
    #[derive(Debug, Clone, Parser)]
    struct CliOptions {
        #[clap(flatten)]
        #[clap(help_heading = "HEADING A")]
        options_a: OptionsA,
    }

    #[derive(Debug, Clone, Args)]
    struct OptionsA {
        #[clap(long)]
        should_be_in_section_a: Option<u32>,
    }

    let app = CliOptions::into_app();

    let should_be_in_section_a = app
        .get_arguments()
        .find(|a| a.get_name() == "should-be-in-section-a")
        .unwrap();
    assert_eq!(should_be_in_section_a.get_help_heading(), Some("HEADING A"));
}

```


### Steps to reproduce the bug with the above code

Add as test and run it

### Actual Behaviour

```
---- flattener_set_help_heading stdout ----
thread 'flattener_set_help_heading' panicked at 'assertion failed: `(left == right)`
  left: `None`,
 right: `Some("HEADING A")`', clap_derive/tests/help.rs:184:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

### Expected Behaviour

Passes

### Additional Context

In providing [clap-verbosity-flag](https://github.com/rust-cli/clap-verbosity-flag), I don't want to prescribe for users what help heading my arguments go under.  At the moment, the only way they can set it is if they set it for the entire struct.

Allowing this argument during flattening would be a big help

### Debug Output

_No response_

---

_Label `T: bug` added by @epage on 2021-10-23 16:09_

---

_Label `C: derive macros` added by @epage on 2021-10-23 16:09_

---

_Comment by @epage on 2021-10-23 16:12_

In fixing this, we'll be introducing a different bug, if both the field and flattened-struct provide `help_heading`, the flattened-struct will win.  We can't fix this without overcomplicating the interface.  I do think allowing to set help_heading when flattening is worth it enough to have that bug.  In general, I'd recommend crates that expose an `Args` to not set `help_heading`.

---

_Referenced in [clap-rs/clap#2930](../../clap-rs/clap/pulls/2930.md) on 2021-10-23 16:17_

---

_Closed by @bors[bot] on 2021-10-25 00:40_

---
