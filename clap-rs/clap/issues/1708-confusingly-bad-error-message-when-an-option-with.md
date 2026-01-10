---
number: 1708
title: "Confusingly bad error message when an option with mutiple values is followed by a subcommand with it's own values"
type: issue
state: closed
author: CreepySkeleton
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2020-02-29T09:15:39Z
updated_at: 2021-09-02T13:39:59Z
url: https://github.com/clap-rs/clap/issues/1708
synced_at: 2026-01-10T01:27:03Z
---

# Confusingly bad error message when an option with mutiple values is followed by a subcommand with it's own values

---

_Issue opened by @CreepySkeleton on 2020-02-29 09:15_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

`1.41.0`

### Affected Version of `clap*`

`master`


### Code
```rust
use clap::Clap;

#[derive(Clap, Debug)]
struct Opts {
    #[clap(subcommand)]
    sub: Subcommands,

    #[clap(long, required = true)]
    files: Vec<String>,
}

#[derive(Clap, Debug)]
enum Subcommands {
    Foo {
        #[clap(long)]
        option_for_subcommand: bool
    }
}

fn main() {
    let opt = Opts::parse();
    println!("{:#?}", opt);
}
```

### Steps to reproduce

`cargo run -- --files file1 file2 foo --option-for-subcommand`

### Actual Behavior Summary

```
error: Found argument '--option-for-subcommand' which wasn't expected, or isn't valid in this context
If you tried to supply `--option-for-subcommand` as a PATTERN use `-- --option-for-subcommand`

USAGE:
    lucid.exe --files <files>... <SUBCOMMAND>

For more information try --help
error: process didn't exit successfully: `target\debug\lucid.exe --files file1 file2 subcommand --option-for-subcommand` (exit code: 2)
```

### Expected Behavior Summary

Something like 
```
error: Found argument '--option-for-subcommand' which wasn't expected, or isn't valid in this context

Hint: an option taking multiple values (--files) is seemingly followed by a subcommand (foo) 
which is not allowed. CLI parser is confused and doesn't know where the values end 
and where the subcommand starts.

USAGE:
    lucid.exe --files <files>... <SUBCOMMAND>

For more information try --help
error: process didn't exit successfully: `target\debug\lucid.exe --files file1 file2 subcommand --option-for-subcommand` (exit code: 2)
```



---

_Label `T: bug` added by @CreepySkeleton on 2020-02-29 09:15_

---

_Label `C: help message` added by @CreepySkeleton on 2020-02-29 09:16_

---

_Label `C: options` added by @CreepySkeleton on 2020-02-29 09:16_

---

_Label `C: subcommands` added by @CreepySkeleton on 2020-02-29 09:16_

---

_Referenced in [clap-rs/clap#1721](../../clap-rs/clap/issues/1721.md) on 2020-03-04 16:56_

---

_Referenced in [clap-rs/clap#1722](../../clap-rs/clap/issues/1722.md) on 2020-03-04 17:02_

---

_Referenced in [TeXitoi/structopt#367](../../TeXitoi/structopt/issues/367.md) on 2020-03-31 12:58_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 08:18_

---

_Comment by @ldm0 on 2020-12-08 15:17_

Current: 
```
error: Found argument '--option-for-subcommand' which wasn't expected, or isn't valid in this context

        If you tried to supply `--option-for-subcommand` as a value rather than a flag, use `-- --option-for-subcommand`

USAGE:
    rust_test.exe --files <files>... <SUBCOMMAND>

For more information try --help
```

---

_Comment by @ldm0 on 2021-08-29 12:07_

The main reason is that the subcommand is treated as a value. [`AppSettings::SubcommandPrecedenceOverArg`](https://docs.rs/clap/3.0.0-beta.4/clap/enum.AppSettings.html#variant.SubcommandPrecedenceOverArg) solves the problem. But it's enabled by the developer rather than user. I don't think the doing the hinting is reasonable.
```rust
use clap::Clap;
use clap::AppSettings;

#[derive(Clap, Debug)]
#[clap(setting=AppSettings::SubcommandPrecedenceOverArg)]
struct Opts {
    #[clap(subcommand)]
    sub: Subcommands,

    #[clap(long, required = true)]
    files: Vec<String>,
}

#[derive(Clap, Debug)]
enum Subcommands {
    Foo {
        #[clap(long)]
        option_for_subcommand: bool
    }
}

fn main() {
    let opt = Opts::parse();
    println!("{:#?}", opt);
}

```

---

_Closed by @ldm0 on 2021-09-02 13:39_

---
