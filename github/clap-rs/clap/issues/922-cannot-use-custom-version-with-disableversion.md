---
number: 922
title: Cannot use custom --version with DisableVersion
type: issue
state: closed
author: anna-is-cute
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2017-04-04T08:06:23Z
updated_at: 2018-08-02T03:30:05Z
url: https://github.com/clap-rs/clap/issues/922
synced_at: 2026-01-07T13:12:19-06:00
---

# Cannot use custom --version with DisableVersion

---

_Issue opened by @anna-is-cute on 2017-04-04 08:06_

### Rust Version

`rustc 1.18.0-nightly (5e122f59b 2017-04-01)`

### Affected Version of clap

`clap 2.22.2`

### Expected Behavior Summary

Overriding --version should allow use of a custom version flag.

### Actual Behavior Summary

The default version provided by clap is used.

### Steps to Reproduce the issue

Specify an `Arg` with `short("v")` and `long("version")`.

### Sample Code or Link to Sample Code

```rust
extern crate clap;

use clap::{App, Arg, AppSettings};

fn main() {
  let matches = App::new("clap-bug")
    .setting(AppSettings::DisableVersion)
    .arg(Arg::with_name("version")
      .short("v")
      .long("version")
      .help("custom version"))
    .get_matches();

  println!("{}", matches.is_present("version"));
}
```

### Debug output

[Output](https://gist.github.com/jkcclemens/8fbcdb1f29259fcf81ddd4b408f830bd)

---

_Renamed from "Cannot use custom --version (but can use custom -v)" to "Cannot use custom --version with DisableVersion" by @anna-is-cute on 2017-04-04 08:16_

---

_Comment by @kbknapp on 2017-04-04 15:40_

Thanks for taking the time to file this! I do have a question however; why are you using `DisableVersion`? You can override the default `--version` by only supplying an arg with a long of `version`.

As I understand it, and please correct me if I'm wrong, this would be an area where I could improve the docs.

---

_Label `T: RFC / question` added by @kbknapp on 2017-04-04 15:41_

---

_Comment by @anna-is-cute on 2017-04-04 21:52_

From what I understood, you needed to use `DisableVersion` to override the default `--version`, but now I know that it's not necessary! I agree that a note in the docs would be helpful to clear any misunderstandings.

I can still reproduce the issue when not using `DisableVersion`, though.

---

_Comment by @kbknapp on 2017-04-04 23:29_

Ah ok, then that *is* a bug!

---

_Label `C: parsing` added by @kbknapp on 2017-04-04 23:30_

---

_Label `D: easy` added by @kbknapp on 2017-04-04 23:30_

---

_Label `P2: need to have` added by @kbknapp on 2017-04-04 23:30_

---

_Label `T: bug` added by @kbknapp on 2017-04-04 23:30_

---

_Label `W: 2.x` added by @kbknapp on 2017-04-04 23:30_

---

_Label `T: RFC / question` removed by @kbknapp on 2017-04-04 23:30_

---

_Closed by @homu on 2017-04-05 12:58_

---
