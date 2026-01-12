```yaml
number: 1700
title: repgrep fails to compile
type: issue
state: closed
author: caedmon-kitty
labels: []
assignees: []
created_at: 2020-10-08T00:50:20Z
updated_at: 2020-10-08T00:55:46Z
url: https://github.com/BurntSushi/ripgrep/issues/1700
synced_at: 2026-01-12T16:13:24Z
```

# repgrep fails to compile

---

_@caedmon-kitty_

$ cargo install repgrep

<successful builds for dependencies not shown>

  Compiling repgrep v0.4.3
error[E0603]: module `derive` is private
  --> /home/michaelr/.cargo/registry/src/github.com-1ecc6299db9ec823/repgrep-0.4.3/build.rs:2:11
   |
2  | use clap::derive::IntoApp;
   |           ^^^^^^ this module is private
   |
note: the module `derive` is defined here
  --> /home/michaelr/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.0-beta.2/src/lib.rs:51:1
   |
51 | mod derive;
   | ^^^^^^^^^^^

error[E0599]: no function or associated item named `into_app` found for struct `Args` in the current scope
  --> /home/michaelr/.cargo/registry/src/github.com-1ecc6299db9ec823/repgrep-0.4.3/build.rs:12:25
   |
12 |     let mut app = Args::into_app();
   |                         ^^^^^^^^ function or associated item not found in `Args`
   | 
  ::: /home/michaelr/.cargo/registry/src/github.com-1ecc6299db9ec823/repgrep-0.4.3/src/cli.rs:20:1
   |
20 | pub struct Args {
   | --------------- function or associated item `into_app` not found for this
   |
   = help: items from traits can only be used if the trait is in scope
help: the following trait is implemented but not in scope; perhaps add a `use` for it:
   |
1  | use clap::derive::IntoApp;
   |

error: aborting due to 2 previous errors

Some errors have detailed explanations: E0599, E0603.
For more information about an error, try `rustc --explain E0599`.
error: could not compile `repgrep`.

To learn more, run the command again with --verbose.
warning: build failed, waiting for other jobs to finish...
error: failed to compile `repgrep v0.4.3`, intermediate artifacts can be found at `/tmp/cargo-installwM5zz3`

Caused by:
  build failed
$ rustc --version
rustc 1.43.1



---

_Closed by @caedmon-kitty on 2020-10-08 00:52_

---

_Comment by @BurntSushi on 2020-10-08 00:52_

This repo is for ripgrep. You're trying to install repgrep.

---

_Comment by @caedmon-kitty on 2020-10-08 00:55_

Sorry - googe returned this repository when I searched for repgrep.  I didn't notice until after I entered the issue.

---
