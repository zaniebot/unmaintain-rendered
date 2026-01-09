---
number: 3193
title: "Next-line help is always used despite no long-help being shown (despite `long_about` being set on a subcommand)"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - S-waiting-on-decision
  - A-help
assignees: []
created_at: 2021-12-17T02:08:40Z
updated_at: 2021-12-17T15:54:03Z
url: https://github.com/clap-rs/clap/issues/3193
synced_at: 2026-01-07T13:12:19-06:00
---

# Next-line help is always used despite no long-help being shown (despite `long_about` being set on a subcommand)

---

_Issue opened by @epage on 2021-12-17 02:08_

### Discussed in https://github.com/clap-rs/clap/discussions/3178

### clap version

`master`

### rust version

 `rustc 1.59.0-nightly (404c8471a 2021-12-14)`

### Minimal Reproducible Code

```rust
use clap::StructOpt;

#[derive(StructOpt, Debug)]
pub struct Opts {
    #[structopt(subcommand)]
    pub subcmd: SubCommand,
}

#[derive(StructOpt, Debug)]
pub struct SampleSubcmd {
    testopt: Option<String>,
}

#[derive(StructOpt, Debug)]
pub enum SubCommand {
    /// With just this line everything renders normally
    ///
    /// THIS NEW PARAGRAPH CHANGES FORMATTING
    Subcmd(SampleSubcmd),
}

fn main() {
    let _opts = Opts::from_args();
}
```

### Reproduction steps

```rust
$ cargo run -- --help
```

### Expected Behavior

```
test-clap 0.1.0

USAGE:
    test-clap <SUBCOMMAND>

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    help      Prints this message or the help of the given subcommand(s)
    subcmd    With just this line everything renders normally
```

### Actual Behavior

```
test-clap 

USAGE:
    test-clap <SUBCOMMAND>

OPTIONS:
    -h, --help
            Print help information

SUBCOMMANDS:
    help
            Print this message or the help of the given subcommand(s)
    subcmd
            With just this line everything renders normally
```

### Context

clap2 shows args on next line but not subcommands.

Note that `long_about` is being set but isn't being shown.

Looks like this is a bug in clap3.

This has levels of fixes
1. Ignore `next_line_help` for subcommands to get to clap2's behavior
2. Ignore subcommands when calculating whether to use `next_line_help` since short help has precedence over long help (fixes the issue reported here)
3. Actually respect short vs long with subcommands and show "THIS NEW PARAGRAPH CHANGES FORMATTING" (I worry there is a reason this wasn't done that is eluding me)

The next part to consider is whether
- args and subcommands should infer `next_line_help` separately
- whether we should stop doing global `next_line_help` inferring and leave it to the individual args

---

_Label `C-bug` added by @epage on 2021-12-17 02:08_

---

_Label `S-waiting-on-decision` added by @epage on 2021-12-17 02:08_

---

_Label `A-help` added by @epage on 2021-12-17 02:08_

---

_Comment by @epage on 2021-12-17 02:10_

@pksunkara wanted to check in in case you had any extra context on
- why subcommands give precedence to `about` on `--help`, unlike args 
- why `next_line_help` is being used by subcommands even if we don't show `long_about`

If curious, you can see my [comment in the discussion](https://github.com/clap-rs/clap/discussions/3178#discussioncomment-1830573) for what this looks like in clap2 and clap3 with and without `long_about` being set (`about` is always set)

---

_Added to milestone `3.0` by @epage on 2021-12-17 02:11_

---

_Renamed from "Always newline when long_about is set - feature or bug?" to "Next-line help is always used despite no long-help being shown (despite `long_about` being set on a subcommand)" by @epage on 2021-12-17 02:13_

---

_Comment by @epage on 2021-12-17 02:44_

I guess the reason to unconditionally give precedence to `about` over `long_about` in `--help` is that the subcommands are just giving you a hint of the help and its expected that you then call `--help` on that subcommand.

---

_Referenced in [clap-rs/clap#3196](../../clap-rs/clap/pulls/3196.md) on 2021-12-17 15:38_

---

_Closed by @epage on 2021-12-17 15:54_

---

_Referenced in [clap-rs/clap#3215](../../clap-rs/clap/issues/3215.md) on 2021-12-24 18:05_

---

_Referenced in [clap-rs/clap#3232](../../clap-rs/clap/pulls/3232.md) on 2021-12-30 18:25_

---

_Referenced in [informalsystems/hermes#2275](../../informalsystems/hermes/pulls/2275.md) on 2022-06-28 09:49_

---
