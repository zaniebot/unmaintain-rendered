---
number: 1737
title: Order of arguments looks wrong when using AllowMissingPositional
type: issue
state: closed
author: jgilchrist
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2020-03-09T23:42:02Z
updated_at: 2021-07-31T06:39:23Z
url: https://github.com/clap-rs/clap/issues/1737
synced_at: 2026-01-07T13:12:19-06:00
---

# Order of arguments looks wrong when using AllowMissingPositional

---

_Issue opened by @jgilchrist on 2020-03-09 23:42_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

```
rustc 1.41.1 (f3e1a954d 2020-02-24)
```

### Code

```rust
use clap::{App, Arg};

fn main() {
    let args: Vec<String> = std::env::args_os()
        .map(|x| x.into_string().expect("Invalid argument"))
        .collect();

    let args_as_str: Vec<&str> = args.iter()
        .map(AsRef::as_ref)
        .collect();

    let clap_app = App::new("")
        .setting(clap::AppSettings::AllowMissingPositional)
        .arg(
            Arg::with_name("first")
                .required(false)
                .default_value("default")
                .index(1),
        )
        .arg(
            Arg::with_name("second")
                .required(true)
                .index(2),
        );

    let matches = clap_app.get_matches_from(args);
}
```
### Steps to reproduce the issue

Run `cargo run -- --help`

### Affected Version of `clap*`

```
2.33.0
```

Also tested with `3.0.0-beta.1` which has the same behaviour.

### Actual Behavior Summary

The indexes for parameters `first` and `second` specify that `first` should come first, then `second`. `first` is optional (using `AllowMissingPositional`). However, the usage shows `second` first, then `first`:

```sh
USAGE:
    httpx <second> [first]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

ARGS:
    <first>      [default: default]
    <second>    
```

### Expected Behavior Summary

```sh
USAGE:
    httpx [first] <second>

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

ARGS:
    <first>      [default: default]
    <second>    
```

---

_Label `T: bug` added by @jgilchrist on 2020-03-09 23:42_

---

_Label `C: help message` added by @CreepySkeleton on 2020-03-10 12:16_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 08:22_

---

_Referenced in [ridhoq/af#2](../../ridhoq/af/pulls/2.md) on 2021-02-08 06:48_

---

_Referenced in [clap-rs/clap#2648](../../clap-rs/clap/pulls/2648.md) on 2021-07-31 00:40_

---

_Closed by @pksunkara on 2021-07-31 06:39_

---
