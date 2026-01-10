---
number: 3132
title: "`no_version` have to be marked twice to hide version info of subcommand"
type: issue
state: closed
author: epage
labels:
  - A-derive
assignees: []
created_at: 2021-12-09T16:34:14Z
updated_at: 2021-12-09T16:40:41Z
url: https://github.com/clap-rs/clap/issues/3132
synced_at: 2026-01-10T01:27:34Z
---

# `no_version` have to be marked twice to hide version info of subcommand

---

_Issue opened by @epage on 2021-12-09 16:34_

<a href="https://github.com/loichyan"><img src="https://avatars.githubusercontent.com/u/73006950?v=4" align="left" width="96" height="96" hspace="10"></img></a> **Issue by [loichyan](https://github.com/loichyan)**
_Friday Feb 12, 2021 at 10:36 GMT_
_Originally opened as https://github.com/TeXitoi/structopt/issues/465_

----

In code below, I have to mark `no_version` at both **P1** and **P2** to hide version info of `sub1`.  For subcommands, their version info should be successfully hided when I marked `no_version` anywhere.

```rust
use structopt::clap::AppSettings::DisableVersion;

#[derive(structopt::StructOpt)]
enum Cmd {
    #[structopt(no_version)]  // P1
    Sub1(Sub1),
}

#[derive(structopt::StructOpt)]
#[structopt(
    no_version,  // P2
    global_settings = &[DisableVersion])]
struct Sub1 {}

#[paw::main]
fn main(cmd: Cmd) {
    match cmd {
        Cmd::Sub1(_s1) => {}
    }
}
```

BTW, this sounds a little similar to #324.


---

_Comment by @epage on 2021-12-09 16:34_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Friday Mar 05, 2021 at 13:33 GMT_

----

There is no way of removing the version of a subcommand, as the different enum can't know each others.

The real solution to this problem would be to have no default version handling, and a argument less `version` to find automatically the version number. It would be coherent with authors. But that's a breaking change, and that's not enough to justify a new major version.


---

_Comment by @epage on 2021-12-09 16:38_

version is now opt-in, instead of opt-out.  We also only propagate populated versions.  I believe this is now fixed.

---

_Closed by @epage on 2021-12-09 16:38_

---

_Label `A-derive` added by @epage on 2021-12-09 16:40_

---
