```yaml
number: 2006
title: "Rename ErrorKind::{VersionDisplayed, HelpDisplayed} to present tense"
type: issue
state: closed
author: davidMcneil
labels:
  - C-enhancement
  - M-breaking-change
  - E-help-wanted
  - E-easy
assignees: []
created_at: 2020-07-10T15:41:51Z
updated_at: 2020-07-26T07:58:22Z
url: https://github.com/clap-rs/clap/issues/2006
synced_at: 2026-01-12T16:14:12Z
```

# Rename ErrorKind::{VersionDisplayed, HelpDisplayed} to present tense

---

_@davidMcneil_

Using clap version `2.33.1`. See the following program:

```rust
use clap::*;

fn main() {
    let app = App::new("app")
        .version("1.2.3")
        .arg(Arg::with_name("arg").takes_value(true).long("arg"));

    let matches = app.get_matches_from_safe(&["app", "--version"]);
    println!("\nerror: {:?}", matches);
}
```

The current output I get is:
```
app 1.2.3
error: Err(Error { message: "", kind: VersionDisplayed, info: None })
```

The output I expect is:
```
error: Err(Error { message: "", kind: VersionDisplayed, info: None })
```

The `--help` option behaves as expected.

---

_Label `T: bug` added by @davidMcneil on 2020-07-10 15:41_

---

_Referenced in [clap-rs/clap#2007](../../clap-rs/clap/pulls/2007.md) on 2020-07-10 15:54_

---

_Referenced in [habitat-sh/habitat#7793](../../habitat-sh/habitat/pulls/7793.md) on 2020-07-10 17:14_

---

_Comment by @CreepySkeleton on 2020-07-11 11:58_

This is fixed in 3.0.
```rust
use clap::*;

fn main() {
    let app = App::new("app")
        .version("1.2.3")
        .arg(Arg::new("arg").takes_value(true).long("arg"));

    let matches = app.try_get_matches_from(&["app", "--version"]);
    println!("\nerror: {:#?}", matches);
}
```

```
error: Err(
    Error {
        cause: "",
        message: app 1.2.3
        ,
        kind: VersionDisplayed,
        info: None,
    },
)
```

We do need to rename the kind to `DisplayVersion` (and probably `HelpDisplayed`). I'm re-purposing the issue.

---

_Renamed from "`get_matches_from_safe` still prints version information to stdout" to "Rename ErrorKind::{VersionDisplayed, HelpDisplayed} to present tense" by @CreepySkeleton on 2020-07-11 12:00_

---

_Label `T: bug` removed by @CreepySkeleton on 2020-07-11 12:00_

---

_Label `D: easy` added by @CreepySkeleton on 2020-07-11 12:00_

---

_Label `E: breaking change` added by @CreepySkeleton on 2020-07-11 12:00_

---

_Label `help wanted` added by @CreepySkeleton on 2020-07-11 12:00_

---

_Label `T: enhancement` added by @CreepySkeleton on 2020-07-11 12:00_

---

_Label `Z: good first issue` added by @CreepySkeleton on 2020-07-11 12:00_

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-07-11 12:01_

---

_Referenced in [clap-rs/clap#2021](../../clap-rs/clap/issues/2021.md) on 2020-07-18 13:16_

---

_Comment by @craigpastro on 2020-07-20 01:00_

This looks like something I can probably handle if that is okay? I suppose `HelpDisplayed` should be renamed to `DisplayHelp`?

---

_Referenced in [clap-rs/clap#2027](../../clap-rs/clap/pulls/2027.md) on 2020-07-20 01:29_

---

_Closed by @bors[bot] on 2020-07-26 07:58_

---
