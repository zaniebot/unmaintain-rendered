---
number: 3028
title: "Easy to accidentally use `setting` when `global_setting` is intended"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - M-breaking-change
  - A-builder
  - A-docs
assignees: []
created_at: 2021-11-15T18:08:38Z
updated_at: 2021-12-08T01:26:14Z
url: https://github.com/clap-rs/clap/issues/3028
synced_at: 2026-01-10T01:27:30Z
---

# Easy to accidentally use `setting` when `global_setting` is intended

---

_Issue opened by @epage on 2021-11-15 18:08_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

2.33

### Minimal reproducible code

When supporting a user on https://github.com/TeXitoi/structopt/issues/129, I accidentally gave the following non-functional code, confused as to why it wasn't working:
```rust
use structopt::StructOpt;

#[derive(StructOpt)]
#[structopt(about = "Sum one or more lists of ints.")]
#[structopt(settings = &[structopt::clap::AppSettings::AllowNegativeNumbers])]
enum Cli {
    SumOne {
        #[structopt(short, required = true)]
        a: Vec<isize>,
    },
    SumTwo {
        #[structopt(short, required = true)]
        a: Vec<isize>,
        #[structopt(short, required = true)]
        b: Vec<isize>,
    },
}

fn main() {
    fn sum(l: &[isize]) -> isize {
        l.iter().sum()
    }
    match Cli::from_args() {
        Cli::SumOne { a } => println!("{}", sum(&a)),
        Cli::SumTwo { a, b } => println!("{} {}", sum(&a), sum(&b)),
    }
}
```
when I should have done
```rust
use structopt::StructOpt;

#[derive(StructOpt)]
#[structopt(about = "Sum one or more lists of ints.")]
#[structopt(global_setting = structopt::clap::AppSettings::AllowNegativeNumbers)]
enum Cli {
    SumOne {
        #[structopt(short, required = true)]
        a: Vec<isize>,
    },
    SumTwo {
        #[structopt(short, required = true)]
        a: Vec<isize>,
        #[structopt(short, required = true)]
        b: Vec<isize>,
    },
}

fn main() {
    fn sum(l: &[isize]) -> isize {
        l.iter().sum()
    }
    match Cli::from_args() {
        Cli::SumOne { a } => println!("{}", sum(&a)),
        Cli::SumTwo { a, b } => println!("{} {}", sum(&a), sum(&b)),
    }
}
```

### Steps to reproduce the bug with the above code

Run it: `cargo run -- sum-one -a 1 2 -3`

### Actual Behaviour

Fails with unknown `-3` arg

### Expected Behaviour

Prints `0`

### Additional Context

I think every place I use an `AppSettings` in my programs, I've done this wrong.

The problem with this is that clap mixes subcommand logic  (e.g. SubcommandRequiredElseHelp) with command logic (e.g. DisabledColoredHelp). Using the command logic without `global_setting` is likely a bug but conversely, using subcommand logic with `global_setting` is likely a bug.  I have not looked through all of the settings to see if there are any middle ground cases.

EDIT: There is a third category, command display logic, like `help_heading`.  This should not be treated with `global`.

At minimum, all examples should be updated to use `global_setting` for command-related settings, even if subcommands aren't used.  People will take these examples and apply them to their subcommands or could later extend their command to have subcommands, so we should treat this as the de facto approach.

However, this still requires a lot of communication to the user so they know which to use when.  Alternatively 
1. #2717 
2. Group command functions separate from subcommand functions (or find an API design to have them on separate types)
3. Layer each App's command config below its subcommands, ie propagate all command configuration to subcommands, recursively, if the subcommand does not have an explicit value set (instead that will then get propagated when recursing)
  - For regular values, we just wrap them in `Option` and provide accessor functions that `unwrap_or(...)` so we can track whether it is explicitly set while being ergonomic.  I've found this pattern fairly successful when dealing with layered config ([example 1](https://github.com/crate-ci/cargo-release/blob/master/src/config.rs#L138), [example 2](https://github.com/epage/git-stack/blob/main/src/config.rs#L319)) and args ([example](https://github.com/epage/git-stack/blob/main/src/bin/git-stack/args.rs#L100)).
  - For Settings, we can have a parallel bitflags that tracks whether the value is explicitly set or not.

This approach provides the behavior users expect while giving them the control to do something we didn't predict.

### Debug Output

_No response_

---

_Label `T: bug` added by @epage on 2021-11-15 18:08_

---

_Referenced in [TeXitoi/structopt#129](../../TeXitoi/structopt/issues/129.md) on 2021-11-15 18:09_

---

_Label `E: breaking change` added by @epage on 2021-11-15 18:15_

---

_Label `C: app` added by @epage on 2021-11-15 18:15_

---

_Label `C: docs` added by @epage on 2021-11-15 18:15_

---

_Referenced in [clap-rs/clap#3034](../../clap-rs/clap/issues/3034.md) on 2021-11-18 14:29_

---

_Referenced in [epage/clapng#240](../../epage/clapng/issues/240.md) on 2021-12-06 22:26_

---

_Comment by @pksunkara on 2021-12-08 01:18_

Would the recent doc changes of giving more visibility to `global_setting` close this issue? I assume not, right?

---

_Comment by @epage on 2021-12-08 01:26_

Yes

---

_Closed by @epage on 2021-12-08 01:26_

---
