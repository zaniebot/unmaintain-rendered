---
number: 2040
title: Specifying a default value but no help text results in the default value being pushed one space to the right inside the help screen
type: issue
state: closed
author: mainrs
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2020-07-30T11:33:55Z
updated_at: 2020-08-25T13:58:29Z
url: https://github.com/clap-rs/clap/issues/2040
synced_at: 2026-01-07T13:12:19-06:00
---

# Specifying a default value but no help text results in the default value being pushed one space to the right inside the help screen

---

_Issue opened by @mainrs on 2020-07-30 11:33_

## Output

```text
next-semver 0.0.0
Detect the next semantic release version based on your commit history

USAGE:
    next-semver.exe [FLAGS] [OPTIONS]

FLAGS:
    -h, --help       Prints help information

OPTIONS:
    -f, --from <from>    The branch, commit or tag from where to start the detection process
    -t, --to <to>         [default: HEAD]
```

Take a look at the `-t,--to` option. It's one space too much aligned to the right :)

### Code

```rust
use clap::Clap;

#[derive(Clap, Debug)]
struct Args {
    /// The branch, commit or tag from where to start the detection process.
    ///
    /// If not specified, the tool will automatically use the latest published
    /// semantic version compatible tag (`vX.X.X`).
    #[clap(long, short)]
    from: Option<String>,
    #[clap(long, short, default_value = "HEAD")]
    to: String,
}

fn main() {
	let _args = Args::parse();
}
```

### Steps to reproduce the issue

1. `cargo run -- -h`

### Version

* **Rust**: rustc 1.41.0 (5e1a79984 2020-01-27)
* **Clap**: 3.0.0-beta.1

### Expected Behavior Summary

The default value should be aligned with the help text, i.e. one space to the left.

__Edit:__ I know, it's nothing major but I haven't found an issue on here so I thought it's still worth mentioning :) 

---

_Label `T: bug` added by @mainrs on 2020-07-30 11:33_

---

_Added to milestone `3.1` by @pksunkara on 2020-07-30 11:49_

---

_Label `C: help message` added by @pksunkara on 2020-07-30 11:50_

---

_Label `C: args` added by @pksunkara on 2020-07-30 11:50_

---

_Comment by @knidarkness on 2020-08-24 16:57_

I'm using `clap` for couple of my projects and really liked it so far. So, @pksunkara if you don't mind I could handle this.

---

_Comment by @pksunkara on 2020-08-24 17:00_

You are welcome to go ahead.

---

_Referenced in [clap-rs/clap#2109](../../clap-rs/clap/pulls/2109.md) on 2020-08-25 07:46_

---

_Closed by @bors[bot] on 2020-08-25 13:58_

---
