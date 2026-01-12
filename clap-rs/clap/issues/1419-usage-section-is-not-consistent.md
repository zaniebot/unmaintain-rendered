```yaml
number: 1419
title: usage section is not consistent
type: issue
state: closed
author: samuela
labels:
  - C-bug
  - A-help
  - E-medium
  - E-easy
assignees: []
created_at: 2019-02-19T18:56:19Z
updated_at: 2021-02-14T12:48:05Z
url: https://github.com/clap-rs/clap/issues/1419
synced_at: 2026-01-12T16:14:10Z
```

# usage section is not consistent

---

_@samuela_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version
rustc 1.32.0 (9fda7c223 2019-01-16)

### Affected Version of clap
2.32.0

### Bug or Feature Request Summary
I have a CLI project in a directory named `rust-cli/` and an app named `my-app`:
```rust
fn make_app<'a, 'b>() -> App<'a, 'b> {
  App::new("my-app")
}
```
When running `./run-cli --help` I get
```
$ ./run-cli --help
my-app 

USAGE:
    rust-cli

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
```
but when running `app.print_help()` I get
```
my-app 

USAGE:
    my-app

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
```

### Expected Behavior Summary
The `print_help()` and `--help` outputs to be the same.

### Actual Behavior Summary
They are not the same.


---

_Label `C: help message` added by @CreepySkeleton on 2020-02-01 15:07_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-01 15:07_

---

_Label `T: bug` added by @CreepySkeleton on 2020-02-01 15:07_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 07:49_

---

_Label `help wanted` removed by @pksunkara on 2020-04-09 07:51_

---

_Label `D: easy` added by @pksunkara on 2020-04-09 07:51_

---

_Label `Z: good first issue` added by @pksunkara on 2020-04-09 07:51_

---

_Label `Z: mentored` added by @pksunkara on 2020-04-09 07:51_

---

_Referenced in [clap-rs/clap#2341](../../clap-rs/clap/pulls/2341.md) on 2021-02-12 14:26_

---

_Comment by @pksunkara on 2021-02-14 12:48_

As discussed in #2341, the priority is given as follows:

When `print_help` function is called:

* `App::bin_name`
* `App::name`

When `--help` is used:

* `App::bin_name`
* executable name of the binary
* `App::name`

If you want consistent behaviour, you need to set `bin_name`

---

_Closed by @pksunkara on 2021-02-14 12:48_

---
