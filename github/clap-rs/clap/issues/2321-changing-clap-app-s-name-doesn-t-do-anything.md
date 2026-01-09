---
number: 2321
title: "Changing Clap App's name doesn't do anything"
type: issue
state: closed
author: henryboisdequin
labels:
  - C-bug
assignees: []
created_at: 2021-01-31T06:26:48Z
updated_at: 2021-01-31T06:33:35Z
url: https://github.com/clap-rs/clap/issues/2321
synced_at: 2026-01-07T13:12:19-06:00
---

# Changing Clap App's name doesn't do anything

---

_Issue opened by @henryboisdequin on 2021-01-31 06:26_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Code

```rust
use clap::{App, Arg, SubCommand};
fn main {
let version = "0.1.0";
let app = App::new("foobar")
        .version(version)
        .author("Henry Boisdequin");
}
```

### Steps to reproduce the issue

1. Run `cargo run`

### Version

* **Rust**: rustc 1.51.0-nightly
* **Clap**: "2.33.3"

### Behavior

I ran this code normally with `cargo run`, everything worked fine. I renamed the app to `foo`:

```rust
use clap::{App, Arg, SubCommand};
fn main {
let version = "0.1.0";
let app = App::new("foo")
        .version(version)
        .author("Henry Boisdequin");
}
```

Then when I run the help command which shows my apps usage it outputs:

```
foo 0.1.0
Henry Boisdequin

USAGE:
    foobar [SUBCOMMAND]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
```

As you can see, the app's name is correctly changed next to the version but in the `USAGE` section, it still says `foobar`. 


---

_Label `T: bug` added by @henryboisdequin on 2021-01-31 06:26_

---

_Comment by @ldm0 on 2021-01-31 06:32_

This is not a bug. If you find your executable, it's name is `foobar` rather than `foo`.

---

_Closed by @henryboisdequin on 2021-01-31 06:33_

---
