---
number: 1990
title: AllowMissingPositional + trailing multiple(true) arg imposes weird requirement
type: issue
state: closed
author: theastrallyforged
labels:
  - C-bug
assignees: []
created_at: 2020-06-28T03:34:51Z
updated_at: 2020-06-30T23:26:48Z
url: https://github.com/clap-rs/clap/issues/1990
synced_at: 2026-01-07T13:12:19-06:00
---

# AllowMissingPositional + trailing multiple(true) arg imposes weird requirement

---

_Issue opened by @theastrallyforged on 2020-06-28 03:34_

### Make sure you completed the following tasks

- [*] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [*] Searched the closes issues

### Code

```rust
use clap::{App, AppSettings, Arg};

fn main() {
    let args = App::new("mcve")
                       .setting(AppSettings::TrailingVarArg)
                       .setting(AppSettings::AllowMissingPositional)
                       .arg(Arg::with_name("file")
                            .required(true))
                       .arg(Arg::with_name("inner-args")
                            .multiple(true))
                       .get_matches();
    dbg!(args);
}
```

### Steps to reproduce the issue

1. `cargo run` (see actual and expected behavior sections)

### Version

* **Rust**: `rustc 1.44.0 (49cae5576 2020-06-01)`
* **Clap**: `2.33.1`

### Actual Behavior Summary

a. `cargo run -- file` => `error: The following required arguments were not provided: <file>`
b. `cargo run -- file -- arg` => `error: The following required arguments were not provided: <file>`
c. `cargo run -- file foo -- bar` => `file` is `"file"` and `inner-args` is `["foo", "--", "bar"]`

### Expected Behavior Summary

a. Parsing succeeds; `file` is `"file"` and `inner-args` is `[]`
b. Parsing succeeds; `file` is `"file"` and `inner-args` is `["arg"]`
c. `file` is `"file"` and `inner-args` is `["foo", "bar"]`

### End Goal

I'm writing an interpreter. The first positional argument is the path to the program being run. Any remaining positional arguments are passed through as arguments to the program, but these program arguments are _optional._ (There are also various flags that control the behavior of the interpreter, but those didn't seem relevant to the main issue.)

I want the usage to look like `interpreter [FLAGS ...] <program> [--] [program-args ...]`. That is:

1. `program`, mandatory positional argument
2. flags, optional non-positional
3. an optional `--` to explicitly begin `program-args`
4. `program-args`, optional positional arguments
5. anything after the first `program-args` is also part of `program-args`, even if it looks like a flag or whatever

`Arg::last()` seems related to what I want, but it makes the `--` _required_, and I want it to be optional.



---

_Label `T: bug` added by @theastrallyforged on 2020-06-28 03:34_

---

_Comment by @CreepySkeleton on 2020-06-29 05:06_

`AllowMissingPositional` is often a bad idea. It's just a workaround for some use cases needed because clap's parser is too dumb to populate positional args properly.

Without it, [everything's working pretty much as expected](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=e4a4a6dcfb0972a2ffedeecae76acf63)

---

_Closed by @pksunkara on 2020-06-30 23:26_

---
