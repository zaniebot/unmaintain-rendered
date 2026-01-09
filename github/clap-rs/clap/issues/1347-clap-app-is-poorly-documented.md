---
number: 1347
title: "clap_app! is poorly documented"
type: issue
state: closed
author: noyez
labels:
  - C-enhancement
  - A-docs
  - E-help-wanted
assignees: []
created_at: 2018-09-27T18:06:51Z
updated_at: 2020-04-12T22:28:26Z
url: https://github.com/clap-rs/clap/issues/1347
synced_at: 2026-01-07T13:12:19-06:00
---

# clap_app! is poorly documented

---

_Issue opened by @noyez on 2018-09-27 18:06_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

`rustc 1.28.0 (9634041f0 2018-07-30)`

### Affected Version of clap

`clap = { version = "2.32", features = [ "suggestions", "color" ] }`

### Bug or Feature Request Summary

Bug

Prelude: this is probably a limitation on Marcos in rust since i notice that with other features of clap, which, btw, clap is awsome). 

When using the `clap_app!` macro, it appears that possible_value cannot be used with integers, for example the following works ([playground link](https://play.rust-lang.org/?gist=ebcda7898ee3392935b46aeee291671c&version=stable&mode=debug&edition=2015)),

```
  #[macro_use] extern crate clap;

fn main() {
    let matches = clap_app!(appname =>
        (@arg numbers: -n --numbers +takes_value possible_value[zero]
            "number")
    ).get_matches();
}
```

However, the following does not compile (not the possible_value option) ([playground link](https://play.rust-lang.org/?gist=6a0b550d49b1f200dc691a2ed662a6a7&version=stable&mode=debug&edition=2015))
```
#[macro_use] extern crate clap;

fn main() {
    let matches = clap_app!(appname =>
        (@arg numbers: -n --numbers +takes_value possible_value[0]
            "number")
    ).get_matches();
}
```
### Expected Behavior Summary

To compile. 

### Actual Behavior Summary

Compile error

```
error: no rules expected the token `arg`
 --> src/main.rs:4:19
  |
4 |       let matches = clap_app!(appname =>
  |  ___________________^
5 | |         (@arg numbers: -n --numbers +takes_value possible_value[0]
6 | |             "number")
7 | |     ).get_matches();
  | |_____^
  |
  = note: this error originates in a macro outside of the current crate (in Nightly builds, run with -Z external-macro-backtrace for more info)
```


### Sample Code or Link to Sample Code
[Compiler Error Playground](https://play.rust-lang.org/?gist=6a0b550d49b1f200dc691a2ed662a6a7&version=stable&mode=debug&edition=2015)

[Comparison code](https://play.rust-lang.org/?gist=ebcda7898ee3392935b46aeee291671c&version=stable&mode=debug&edition=2015)


---

_Comment by @mmastrac on 2018-11-05 23:43_

This syntax is poorly documented, but should work:

Playground link: https://play.rust-lang.org/?version=stable&mode=debug&edition=2015&gist=7250c15220ae9126792f52999758306c

```
#[macro_use] extern crate clap;

fn main() {
    let matches = clap_app!(appname =>
        (@arg numbers: -n --numbers +takes_value possible_value("0")
            "number")
    ).get_matches();
}
```

---

_Renamed from "Using macro with possible_value[0] does not compile" to "clap_app! is poorly documented" by @CreepySkeleton on 2020-02-02 05:15_

---

_Label `C: docs` added by @CreepySkeleton on 2020-02-02 05:16_

---

_Label `C: macros` added by @CreepySkeleton on 2020-02-02 05:16_

---

_Label `T: enhancement` added by @CreepySkeleton on 2020-02-02 05:16_

---

_Label `W: 2.x` added by @CreepySkeleton on 2020-02-02 05:16_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-02 05:16_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-02 05:16_

---

_Added to milestone `3.1` by @CreepySkeleton on 2020-02-02 05:16_

---

_Referenced in [clap-rs/clap#1813](../../clap-rs/clap/pulls/1813.md) on 2020-04-12 10:03_

---

_Removed from milestone `3.1` by @pksunkara on 2020-04-12 10:04_

---

_Added to milestone `3.0` by @pksunkara on 2020-04-12 10:04_

---

_Closed by @bors[bot] on 2020-04-12 22:28_

---
