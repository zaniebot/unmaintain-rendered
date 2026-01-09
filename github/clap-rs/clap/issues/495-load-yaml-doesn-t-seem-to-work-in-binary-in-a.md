---
number: 495
title: "load_yaml! doesn't seem to work in binary in a library project."
type: issue
state: closed
author: locallycompact
labels: []
assignees: []
created_at: 2016-05-05T19:01:20Z
updated_at: 2018-08-02T03:29:49Z
url: https://github.com/clap-rs/clap/issues/495
synced_at: 2026-01-07T13:12:19-06:00
---

# load_yaml! doesn't seem to work in binary in a library project.

---

_Issue opened by @locallycompact on 2016-05-05 19:01_

with features = ["yaml"] enabled, this doesn't seem to propagate to binaries in a library project.

Cargo.toml:

```
[package]
name = "foo"
version = "0.1.0"
authors = ["Daniel Firth <dan.firth@codethink.co.uk>"]

[dependencies]
clap = { features = ["yaml"] }
```

src/lib.rs

```
#[macro_use]

mod foo {
}
```

src/bin/bar.rs

```
#[macro_use]

extern crate foo;
extern crate clap;

use clap::App;

fn main() {
    let yaml = load_yaml!("cli.yml");
}
```

Results in

```
src/bin/bar.rs:10:16: 10:25 error: macro undefined: 'load_yaml!'
src/bin/bar.rs:10     let yaml = load_yaml!("cli.yml");
```

The project is here for convenience: https://github.com/locallycompact/foo-clap


---

_Closed by @locallycompact on 2016-05-05 19:16_

---

_Comment by @ghost on 2017-11-09 17:11_

It works if you use

```
#[macro_use]

extern crate clap;
extern crate foo;
```

instead of

```
#[macro_use]

extern crate foo;
extern crate clap;
```

---
