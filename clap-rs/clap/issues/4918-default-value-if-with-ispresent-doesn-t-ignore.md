```yaml
number: 4918
title: "`default_value_if` with `IsPresent` doesn't ignore defaults"
type: issue
state: open
author: epage
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2023-05-18T18:54:01Z
updated_at: 2023-05-18T18:54:02Z
url: https://github.com/clap-rs/clap/issues/4918
synced_at: 2026-01-12T16:14:16Z
```

# `default_value_if` with `IsPresent` doesn't ignore defaults

---

_@epage_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.69

### Clap Version

4.2.7

### Minimal reproducible code

```rust
    /// ```rust
    /// # use clap_builder as clap;
    /// # use clap::{Command, Arg, ArgAction};
    /// # use clap::builder::{ArgPredicate};
    /// let m = Command::new("prog")
    ///     .arg(Arg::new("flag")
    ///         .long("flag")
    ///         .action(ArgAction::SetTrue))
    ///     .arg(Arg::new("other")
    ///         .long("other")
    ///         .default_value_if("flag", ArgPredicate::IsPresent, Some("default")))
    ///     .get_matches_from(vec![
    ///         "prog"
    ///     ]);
    ///
    /// assert_eq!(m.get_one::<String>("other"), None);
    /// ```

```


### Steps to reproduce the bug with the above code

Put that in the docs

### Actual Behaviour

Panic

### Expected Behaviour

Passes

### Additional Context

Found via #4904

Holding off until clap v5 in case this is disruptive

### Debug Output

_No response_

---

_Label `C-bug` added by @epage on 2023-05-18 18:54_

---

_Label `A-parsing` added by @epage on 2023-05-18 18:54_

---

_Added to milestone `5.0` by @epage on 2023-05-18 18:54_

---

_Referenced in [clap-rs/clap#4904](../../clap-rs/clap/issues/4904.md) on 2023-05-18 18:57_

---

_Referenced in [clap-rs/clap#4938](../../clap-rs/clap/issues/4938.md) on 2023-05-24 18:59_

---
