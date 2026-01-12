```yaml
number: 2086
title: "Problem with `clap::App::author(crate_authors!(\", \"))` when called from function"
type: issue
state: closed
author: finga
labels:
  - C-bug
assignees: []
created_at: 2020-08-20T13:45:14Z
updated_at: 2020-08-21T09:32:39Z
url: https://github.com/clap-rs/clap/issues/2086
synced_at: 2026-01-12T16:14:12Z
```

# Problem with `clap::App::author(crate_authors!(", "))` when called from function

---

_@finga_

I wanted to tighten up a `main()` function and therefor tried to move the `clapp::App::new(...` to another function in another file but this does not seem to work when using `crate_authors!(", ")`

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Code
#### Working code before moving
##### `main.rs`
```rust
fn main() {
...
let args = App::new(env!("CARGO_PKG_NAME"))
        .version(env!("CARGO_PKG_VERSION"))
        .author(crate_authors!(", "))
        .about(env!("CARGO_PKG_DESCRIPTION"))
        .arg(some_arg)
        .subcommand(some_subcommand)
        .get_matches();
...
}
```
#### Code I wanted to have but I could not get to work no matter if I use `&`, `[..]` etc..
##### `cli.rs`
```rust
pub fn args() -> App<'static> {
...
App::new(env!("CARGO_PKG_NAME"))
        .version(env!("CARGO_PKG_VERSION"))
        .author(crate_authors!(", ")) // using `crate_authors!()` still works
        .about(env!("CARGO_PKG_DESCRIPTION"))
        .arg(some_arg)
        .subcommand(some_subcommand)
```
##### `main.rs`
```rust
fn main() {
...
let args = cli::args().get_matches();
...
}
```

### Version

* **Rust**: rustc 1.45.0 (5c1f21c3b 2020-07-13)
* **Clap**: 3.0.0-beta.1

### Actual Behavior Summary
I get a warning that the `.author(crate_authors(", "))` line creates a temporary value and that the whole `App::new(...` call returns a value referencing data owned by the current function.

### Expected Behavior Summary
To print the help message as when `App::new(...` is inside the `main()` function.

### Additional context
I hope this is enough content and context if not let me know.

---

_Label `T: bug` added by @finga on 2020-08-20 13:45_

---

_Added to milestone `3.0` by @pksunkara on 2020-08-20 17:42_

---

_Comment by @CreepySkeleton on 2020-08-21 05:10_

Lol, this macro doesn't even do the caching _right_. We'll have to use `lazy_static` there. 

---

_Referenced in [clap-rs/clap#2092](../../clap-rs/clap/pulls/2092.md) on 2020-08-21 05:27_

---

_Closed by @bors[bot] on 2020-08-21 09:32_

---
