---
number: 4731
title: Add logic to ArgPredicate
type: issue
state: open
author: jaskij
labels:
  - C-enhancement
assignees: []
created_at: 2023-02-27T04:03:48Z
updated_at: 2023-02-27T17:09:49Z
url: https://github.com/clap-rs/clap/issues/4731
synced_at: 2026-01-10T01:28:00Z
---

# Add logic to ArgPredicate

---

_Issue opened by @jaskij on 2023-02-27 04:03_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.1.6

### Describe your use case

I want to set an arguments default value, using `default_value_ifs()` depending on multiple other arguments.

For example, in the code below, I would like to change the value for destination depending on the value of `data_type` (since in actual usage the UDP port depends on the type of data we're sending).

```rust
#[derive(Debug, clap::Parser)]
#[command(
    long_about = None,
    long_version(crate::build_info::BUILD_INFO.long_version_string(),
))]
pub(crate) struct Cli {
    #[arg(value_enum)]
    // Which data type to send
    pub(crate) data_type: DataType,

    #[arg(value_enum)]
    // How to send
    pub(crate) sender: Sender,

    #[arg(
        short,
        long,
        default_value_ifs([
            ("sender", clap::builder::ArgPredicate::Equals("nats".into()), "localhost:4222"),
            ("sender", clap::builder::ArgPredicate::Equals("udp".into()), "localhost:8585")
        ]),
    )]
    /// Where to send
    pub destination: String,
}
```

### Describe the solution you'd like

The idea is to expand `ArgPredicate` to look something like this:

```rust
pub enum ArgPredicate {
    IsPresent,
    Equals(OsStr),
    And(ArgPredicate, ArgPredicate),
    Or(ArgPredicate, ArgPredicate),
}
```

### Alternatives, if applicable

No idea

### Additional Context

_No response_

---

_Label `C-enhancement` added by @jaskij on 2023-02-27 04:03_

---

_Comment by @epage on 2023-02-27 16:13_

We've talked of generalizing a lot of this logic in #3476.

For this specific proposal, I'm not seeing how it would work as it sounds like you want a default for `destination` to read both `sender` and `data_type` but `ArgPredicate` doesn't support that, so adding `And` and `Or` wouldn't do much.

---

_Comment by @jaskij on 2023-02-27 17:09_

Argh, my bad. Yes, it does not read multiple arguments. So the change would require moving the argument name into the predicate, like so:

```rust
pub enum ArgPredicate {
    IsPresent,
    Equals(OsStr, OsStr),
    And(ArgPredicate, ArgPredicate),
    Or(ArgPredicate, ArgPredicate),
}
```

But, I just realized that this would lead to a full-blown logic system and end up with an [inner platform effect](https://thedailywtf.com/articles/the_inner-platform_effect). So might as wall ditch the predicate, accept something like `Fn(&HashMap<OsStr, OsStr>) -> OsStr` and let clap's user worry about it.

---

_Referenced in [clap-rs/clap#3008](../../clap-rs/clap/issues/3008.md) on 2023-05-03 14:14_

---
