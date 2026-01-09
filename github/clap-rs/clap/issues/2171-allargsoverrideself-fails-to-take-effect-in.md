---
number: 2171
title: AllArgsOverrideSelf fails to take effect in certain patterns of arguments
type: issue
state: closed
author: ssokolow
labels:
  - C-bug
  - A-parsing
  - ":money_with_wings: $5"
assignees: []
created_at: 2020-10-13T10:53:02Z
updated_at: 2021-05-31T09:28:32Z
url: https://github.com/clap-rs/clap/issues/2171
synced_at: 2026-01-07T13:12:19-06:00
---

# AllArgsOverrideSelf fails to take effect in certain patterns of arguments

---

_Issue opened by @ssokolow on 2020-10-13 10:53_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Code

```rust
use clap::{Arg, App, AppSettings};

fn main() {
    let schema = App::new("ripgrep#1701 reproducer")
        .setting(AppSettings::AllArgsOverrideSelf)
        .arg(Arg::with_name("pretty")
            .short("p")
            .long("pretty"))
        .arg(Arg::with_name("search_zip")
            .short("z")
            .long("search-zip"));

    let test_args = &[
       vec!["reproducer", "-pz", "-p"],
       vec!["reproducer", "-pzp"],
       vec!["reproducer", "-zpp"],
       vec!["reproducer", "-pp", "-z"],
       vec!["reproducer", "-p", "-p", "-z"],

       vec!["reproducer", "-p", "-pz"],
       vec!["reproducer", "-ppz"],
    ];


    for argv in test_args {
        let matches = schema.clone().get_matches_from_safe(argv);
        match matches {
            Ok(_) => println!(" OK: {:?}", argv),
            Err(e) => println!("ERR: {:?} ({:?})", argv, e.kind),
        }
    }
}
```

### Steps to reproduce the issue

1. Run `cargo run` for the reproducer script

### Version

* **Rust**: `rustc 1.47.0 (18bf6b4f0 2020-10-07)`
* **Clap**: 2.33.3

### Actual Behavior Summary

```
 OK: ["reproducer", "-pz", "-p"]
 OK: ["reproducer", "-pzp"]
 OK: ["reproducer", "-zpp"]
 OK: ["reproducer", "-pp", "-z"]
 OK: ["reproducer", "-p", "-p", "-z"]
ERR: ["reproducer", "-p", "-pz"] (UnexpectedMultipleUsage)
ERR: ["reproducer", "-ppz"] (UnexpectedMultipleUsage)
```

I tripped over this when a script that invoked `rg -pz` errored out because I had `--pretty` in my ripgrep configuration file.

### Expected Behavior Summary

All of the strings in the reproducer should behave identically.

### Additional context

[debug.log](https://github.com/clap-rs/clap/files/5370816/debug.log)

---

_Label `T: bug` added by @ssokolow on 2020-10-13 10:53_

---

_Referenced in [BurntSushi/ripgrep#1701](../../BurntSushi/ripgrep/issues/1701.md) on 2020-10-13 10:53_

---

_Label `C: parsing` added by @pksunkara on 2020-10-13 10:59_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2020-10-13 10:59_

---

_Added to milestone `3.0` by @pksunkara on 2020-10-13 10:59_

---

_Removed from milestone `3.0` by @pksunkara on 2020-10-23 09:31_

---

_Added to milestone `3.1` by @pksunkara on 2020-10-23 09:31_

---

_Comment by @ldm0 on 2021-01-27 17:38_

Linked to #1374, should be fixed by #2297, add regress tests later.

---

_Referenced in [clap-rs/clap#2297](../../clap-rs/clap/pulls/2297.md) on 2021-01-27 17:43_

---

_Comment by @ldm0 on 2021-01-27 17:44_

Done.

---

_Closed by @pksunkara on 2021-02-07 15:11_

---

_Comment by @BurntSushi on 2021-05-31 01:44_

@ldm0 Did this make it into a clap 2.x release? Or is it only fixed in clap 3.x?

---

_Comment by @pksunkara on 2021-05-31 09:28_

Only in 3.x

---

_Referenced in [commercial-emacs/commercial-emacs#7](../../commercial-emacs/commercial-emacs/pulls/7.md) on 2021-09-06 02:02_

---

_Referenced in [commercial-emacs/commercial-emacs#27](../../commercial-emacs/commercial-emacs/pulls/27.md) on 2021-09-06 14:26_

---

_Referenced in [commercial-emacs/commercial-emacs#39](../../commercial-emacs/commercial-emacs/pulls/39.md) on 2021-09-06 20:54_

---

_Referenced in [commercial-emacs/commercial-emacs#62](../../commercial-emacs/commercial-emacs/pulls/62.md) on 2021-09-08 13:54_

---
