---
number: 4191
title: Context-Aware Multiple Usage from Argument Groups
type: issue
state: open
author: andersonjwan
labels:
  - C-enhancement
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2022-09-07T19:11:02Z
updated_at: 2025-12-19T16:01:55Z
url: https://github.com/clap-rs/clap/issues/4191
synced_at: 2026-01-07T13:12:20-06:00
---

# Context-Aware Multiple Usage from Argument Groups

---

_Issue opened by @andersonjwan on 2022-09-07 19:11_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.2.20

### Describe your use case

Currently, the only method, as far as I am aware, to generate a USAGE section with more than two usage statements from a set of grouped positional arguments is to explicitly pass a string to `clap::builder::Command::override_usage`.

However, opting-in for this solution disables the useful context-aware capabilities that the library has to offer. For example, if a new set of positional arguments are added, the resulting USAGE section does not reflect these changes without explicitly adjusting the call to `override_usage`.

To provide a small working example, a small demonstration of the `grep` tool usage without overriding the usage section would be as follows:

```rust
fn main() {
    Command::new("jrep")
        .arg(
            Arg::new("BASIC")
                .required(true)
                .value_name("PATTERNS")
        )
        .arg(
            Arg::new("EXTENDED")
                .required(true)
                .long("regexp")
                .short('e')
                .value_name("PATTERNS"),
        )
        .arg(
            Arg::new("PATTERN FILE")
                .required(true),
                .long("file")
                .short('f')
                .takes_value(true),
        )
        .group(
            ArgGroup::new("pattern-input")
                .arg("BASIC")
                .arg("EXTENDED")
                .arg("PATTERN FILE")
                .required(true),
        )
        .arg(
            Arg::new("FILE")
                .required(true)
                .takes_value(true)
                .multiple_values(true),
        )
        .arg(
            Arg::new("count")
                .long("count")
                .short('c')
                .action(ArgAction::SetTrue),
        )
        .get_matches();
}
```

This results in the following USAGE section:

```bash
USAGE:
    jrep [OPTIONS] <PATTERNS|--regexp <PATTERNS>|--file <PATTERN FILE>> <FILE>...
```

However, the desirable output, only acquirable by overriding the usage string and losing context-aware generation is:

```bash
USAGE:
    jrep [OPTIONS] <PATTERN> <FILE>
    jrep [OPTIONS] --regexp <PATTERNS> <FILE>
    jrep [OPTIONS] --file <PATTERN FILE> <FILE>
```

### Describe the solution you'd like

As a result, grouped arguments (especially positional), should be given the option to generate multiple USAGE statements for every possible combination (i.e., perform the cartesian product over each `ArgGroup`).

The interface for this option could look as follows:

```rust
fn main() {
    Command::new("jrep")
        .display_all_possible_usages(true)
        ...
}
```

Which, would generate the following USAGE section (assuming the same arguments as defined in the previous section):

```bash
USAGE:
    jrep [OPTIONS] <PATTERN> <FILE>
    jrep [OPTIONS] --regexp <PATTERNS> <FILE>
    jrep [OPTIONS] --file <PATTERN FILE> <FILE>
```

### Alternatives, if applicable

The only alternative to this is to use the `clap::build::Command::override_usage` which, as stated above, inhibits the context-aware generation of the USAGE section.

### Additional Context

From my own search, there are only two instances where multiple usage statements are discussed:

1. #3413

The issue above only discusses making the USAGE string more intuitive to write and does not discuss the matter of deriving a USAGE section from the context-aware `ArgGroup`s.

2. #2103

This discussion is over two years old. However, I would argue that the use-case and need for the feature are still relevant and possibly more feasible with the more recent versions.

---

_Label `C-enhancement` added by @andersonjwan on 2022-09-07 19:11_

---

_Label `A-help` added by @epage on 2022-09-07 19:21_

---

_Label `S-waiting-on-design` added by @epage on 2022-09-07 19:21_

---

_Comment by @epage on 2022-09-07 19:29_

> As a result, grouped arguments (especially positional), should be given the option to generate multiple USAGE statements for every possible combination (i.e., perform the cartesian product over each ArgGroup).

Rather than applying a cartesian product across every `ArgGroup`, I think it'd work better if instead we had `group.multiple_usage(true)` (ie you opt-in on a per-group basis.   We'll need to make sure that we render the arg in its dedicated line rather than being elided in `[OPTIONS]` (if its optional).

We'll also want to look at several clap users today that use groups to verify any assumptions of how well this would work and if any tweaks might be needed.

A challenge for this will be balancing a usability improvement like this with our efforts to reduce compile time (https://github.com/clap-rs/clap/issues/2037) and binary size (https://github.com/clap-rs/clap/issues/1365) which are top complaints of clap today.

> Currently, the only method, as far as I am aware, to generate a USAGE section with more than two usage statements from a set of grouped positional arguments is to explicitly pass a string to clap::builder::Command::override_usage.

clap will automatically generate two usage statements if `args_conflicts_with_subcommands` is used ([example](https://github.com/clap-rs/clap/blob/master/tests/builder/help.rs#L1104)).

---

_Comment by @andersonjwan on 2022-09-07 20:00_

>Rather than applying a cartesian product across every `ArgGroup`, I think it'd work better if instead we had `group.multiple_usage(true)` (ie you opt-in on a per-group basis. We'll need to make sure that we render the arg in its dedicated line rather than being elided in `[OPTIONS]` (if its optional).

Agreed. Having per-`ArgGroup` control over generating additional usage statements is a more concise and minimal assumption-based approach for this feature.

I would say, it may be possible to be even more granular and supply a per-argument within a group usage generation option. For example, the method `ArgGroup::usage(mut self, arg_id)` to specify which arguments generate separate usage statements. Of course, the breaking change would be to modify `ArgGroup::arg(..., usage: bool)` to specify during addition of a new argument.

>We'll also want to look at several clap users today that use groups to verify any assumptions of how well this would work and if any tweaks might be needed.

I can perform a preliminary search of this and report back some results here.

>A challenge for this will be balancing a usability improvement like this with our efforts to reduce compile time (https://github.com/clap-rs/clap/issues/2037) and binary size (https://github.com/clap-rs/clap/issues/1365) which are top complaints of clap today.

Noted. Extending the interface of the `ArgGroup` struct to support this option is trivial. However, to generate the actual USAGE section is a bit more work. Do you mind pointing me in the direction where the USAGE section gets generated? I would like to have a look and possibly, if feasible, to provide this feature. Else, if someone is better suited, such as yourself, by all means!


---

_Comment by @epage on 2022-09-07 20:17_

> Do you mind pointing me in the direction where the USAGE section gets generated? 

https://github.com/clap-rs/clap/blob/master/src/output/usage.rs

Tests would either go in
- https://github.com/clap-rs/clap/blob/master/tests/builder/groups.rs
- https://github.com/clap-rs/clap/blob/master/tests/builder/help.rs

---

_Referenced in [clap-rs/clap#4132](../../clap-rs/clap/issues/4132.md) on 2023-07-13 12:43_

---

_Referenced in [clap-rs/clap#5625](../../clap-rs/clap/issues/5625.md) on 2025-02-27 16:48_

---

_Referenced in [rust-lang/cargo#15363](../../rust-lang/cargo/issues/15363.md) on 2025-03-28 19:43_

---

_Comment by @epage on 2025-12-19 16:01_

As pointed out in #6199, we should also support `exclusive` for this

---
