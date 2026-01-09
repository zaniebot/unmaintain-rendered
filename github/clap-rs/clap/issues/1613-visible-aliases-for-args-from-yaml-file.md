---
number: 1613
title: Visible aliases for Args from YAML file 
type: issue
state: closed
author: GrayJack
labels:
  - C-bug
  - ":money_with_wings: $5"
assignees: []
created_at: 2019-12-28T01:05:40Z
updated_at: 2021-01-20T18:10:16Z
url: https://github.com/clap-rs/clap/issues/1613
synced_at: 2026-01-07T13:12:19-06:00
---

# Visible aliases for Args from YAML file 

---

_Issue opened by @GrayJack on 2019-12-28 01:05_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version
rustc 1.40.0 (73528e339 2019-12-16)

### Affected Version of clap
version = "2.33.0"
source = "registry+https://github.com/rust-lang/crates.io-index"
checksum = "5067f5bb2d80ef5d68b4c87db81601f0b75bca627bc2ef76b141d7b846a3c6d9"

### Expected Behavior Summary
When using `visible_alias` on arguments, set a visible alias for that argument, much like it happens for App (command and subcommand

### Actual Behavior Summary
```sh
--- stderr
thread 'main' panicked at 'Unknown Arg setting 'visible_alias' in YAML file for arg '<program-name>'', clap-2.33.0/src/args/arg.rs:149:22
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace.
```

### Steps to Reproduce the issue
 1 - Add for a Arg `visible_alias` on a YAML file.
 2 - Try to compile


### Sample Code or Link to Sample Code
```yaml
name: program
version: "0.0.0"
about: My program
args:
  - flag:
    long: long_flag
    short: l
    visible_alias: another_name
```

```rust
use clap::{load_yaml, App, AppSettings::ColoredHelp, ArgMatches};

fn main() {
    let yaml = load_yaml!("my_yaml.yml");

    let _matches = App::from_yaml(yaml).get_matches();
```


### Debug output
There is no debug output, it errors out at compile time


---

_Label `W: 2.x` added by @CreepySkeleton on 2020-02-01 10:57_

---

_Label `C: alias` added by @CreepySkeleton on 2020-02-01 10:58_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 08:12_

---

_Label `W: 2.x` removed by @pksunkara on 2020-04-09 08:12_

---

_Label `T: bug` added by @pksunkara on 2020-04-09 08:12_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2020-10-08 21:07_

---

_Referenced in [clap-rs/clap#2302](../../clap-rs/clap/pulls/2302.md) on 2021-01-19 20:51_

---

_Closed by @pksunkara on 2021-01-20 18:07_

---

_Comment by @pksunkara on 2021-01-20 18:10_

@AriusX7 You can follow the steps in https://github.com/clap-rs/clap/wiki/Issue-Bounties to get the bounty.

---
