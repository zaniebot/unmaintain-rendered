```yaml
number: 1251
title: unresolved import app
type: issue
state: closed
author: luisbg
labels:
  - invalid
assignees: []
created_at: 2019-04-16T18:17:02Z
updated_at: 2019-04-16T18:36:50Z
url: https://github.com/BurntSushi/ripgrep/issues/1251
synced_at: 2026-01-12T16:13:23Z
```

# unresolved import app

---

_@luisbg_

#### What version of ripgrep are you using?

git clone today right after the 11.0.1

#### How did you install ripgrep?

Building from source.

#### What operating system are you using ripgrep on?

Building on macOS. Strangely this doesn't happen when building in Ubuntu 18.10.

#### Describe your question, feature request, or bug.

When trying to build ripgrep I get the following error.

```
error[E0432]: unresolved import `app`
 --> build.rs:9:5
  |
9 | use app::{RGArg, RGArgKind};
  |     ^^^ Did you mean `self::app`?

error: aborting due to previous error

For more information about this error, try `rustc --explain E0432`.
error: Could not compile `ripgrep`.
```

#### If this is a bug, what are the steps to reproduce the behavior?

cargo build --release

#### If this is a bug, what is the expected behavior?

A clean build :)


---

_Comment by @BurntSushi on 2019-04-16 18:22_

Please show the output of `rustc --version` and `cargo --version`.

---

_Comment by @luisbg on 2019-04-16 18:30_

I should've thought of doing that. Sorry.

Problem happens with nightly, but not with stable.
```
$ rustc --version
rustc 1.31.0-nightly (b2d6ea98b 2018-10-07)

$ cargo --version
cargo 1.31.0-nightly (ad6e5c003 2018-09-28)
```

---

_Comment by @BurntSushi on 2019-04-16 18:34_

As the [README says](https://github.com/BurntSushi/ripgrep#building), ripgrep's minimum supported Rust version is 1.34. So you'll need to upgrade.

---

_Closed by @BurntSushi on 2019-04-16 18:34_

---

_Comment by @luisbg on 2019-04-16 18:35_

Updating nightly fixes the problem. False alarm.

```
$ rustc --version
rustc 1.35.0-nightly (2975a3c4b 2019-04-15)
```

Not sure why I had such an old nightly. I reinstalled this machine recently.

Sorry for distracting you with an invalid issue, and thanks for the quick response.

Hopefully if anybody else sees the same problem this can act as a reference to help them.

---

_Label `invalid` added by @BurntSushi on 2019-04-16 18:35_

---
