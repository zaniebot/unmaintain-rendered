```yaml
number: 1306
title: cargo build error
type: issue
state: closed
author: lucasjinreal
labels:
  - invalid
assignees: []
created_at: 2019-06-19T13:52:08Z
updated_at: 2019-06-19T13:55:25Z
url: https://github.com/BurntSushi/ripgrep/issues/1306
synced_at: 2026-01-12T16:13:23Z
```

# cargo build error

---

_@lucasjinreal_

```
error[E0658]: imports can only refer to extern crate names passed with `--extern` on stable channel (see issue #53130)
  --> /Users/jintian/.cargo/registry/src/mirrors.ustc.edu.cn-12df342d903acd47/ripgrep-11.0.1/build.rs:9:5
   |
9  | use app::{RGArg, RGArgKind};
   |     ^^^
...
13 | mod app;
   | -------- not an extern crate passed with `--extern`
   |
note: this import refers to the module defined here
  --> /Users/jintian/.cargo/registry/src/mirrors.ustc.edu.cn-12df342d903acd47/ripgrep-11.0.1/build.rs:13:1
   |
13 | mod app;
   | ^^^^^^^^

error: aborting due to previous error
```

---

_Comment by @BurntSushi on 2019-06-19 13:55_

Why didn't you include the version of the Rust compiler you're using? If you're going to report a compilation error, then please at least provide all the necessary details to reproduce the problem.

In any case, I'd recommend that you [read the build instructions in the README](https://github.com/BurntSushi/ripgrep#building), and in particular, note the minimum Rust version requirement:

> ripgrep compiles with Rust 1.34.0 (stable) or newer. In general, ripgrep tracks the latest stable release of the Rust compiler.

---

_Closed by @BurntSushi on 2019-06-19 13:55_

---

_Label `invalid` added by @BurntSushi on 2019-06-19 13:55_

---
