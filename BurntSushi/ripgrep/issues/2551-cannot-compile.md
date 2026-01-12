```yaml
number: 2551
title: Cannot compile
type: issue
state: closed
author: aakarshan-raj
labels:
  - invalid
assignees: []
created_at: 2023-07-06T13:46:55Z
updated_at: 2023-07-06T13:52:44Z
url: https://github.com/BurntSushi/ripgrep/issues/2551
synced_at: 2026-01-12T16:13:24Z
```

# Cannot compile

---

_@aakarshan-raj_

#### What version of ripgrep are you using?

not applicable 

#### How did you install ripgrep?

i am compiling

#### What operating system are you using ripgrep on?
linux

#### Describe your bug.

compilers issuse error code[E0658] at 4 instance while compiling and aborts.sighting "use of unstanble feature 'is_terminal'" 

#### What are the steps to reproduce the behavior?
 clone the repo
 cd in to it
 `cargo build --release`



---

_Comment by @BurntSushi on 2023-07-06 13:52_

You're probably using a version of Rust that is too old. I believe you need Rust 1.70.

(The README is unfortunately out of date and so is the `rust-version` in `Cargo.toml`. Once that's updated, you'll get a better error message here.)

If upgrading your Rust toolchain isn't feasible or isn't something you want to do, then you'll need to build an older version of ripgrep. Try `git checkout 13.0.0`.

---

_Closed by @BurntSushi on 2023-07-06 13:52_

---

_Label `invalid` added by @BurntSushi on 2023-07-06 13:52_

---
