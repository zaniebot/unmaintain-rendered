```yaml
number: 1532
title: help_short() allows conflict option
type: issue
state: closed
author: maicallist
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2019-08-22T04:59:32Z
updated_at: 2020-04-10T02:00:15Z
url: https://github.com/clap-rs/clap/issues/1532
synced_at: 2026-01-12T16:14:11Z
```

# help_short() allows conflict option

---

_@maicallist_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

rustc 1.37.0 (eae3437df 2019-08-13)C

### Affected Version of clap

2.33.0

### Bug or Feature Request Summary
help_short() allows conflict option.

### Expected Behavior Summary

when setting arg with short(`h`) and help_short(`h`) 
Clap should give an error about the conflict.

### Actual Behavior Summary

Under App
pre-condition, App has a `-h` arg.
1. when not setting `help_short()`, clap works as expected.
2. when `help_short("h")` is set, `-h` option prints help info and lists `-h` is for two options.
<img width="325" alt="Screen Shot 2019-08-23 at 10 31 08" src="https://user-images.githubusercontent.com/6936908/63562723-4a5c3f00-c591-11e9-8d9b-da74e701159f.png">

Under Subcommand
pre-condition, subcommand has `-h` arg.

1. when not setting `help_short()`, clap works as expected.
2. when `help_short("h")` is set, `-h` option prints nothing and `--help` lists that `-h` is for two options.
<img width="642" alt="Screen Shot 2019-08-23 at 10 20 19" src="https://user-images.githubusercontent.com/6936908/63562288-ac1ba980-c58f-11e9-8222-31e167e46e39.png">


---

_Renamed from "'-h' option under subcommand not working as expected" to "help_short() allows conflict option" by @maicallist on 2019-08-23 02:27_

---

_Label `C: help message` added by @CreepySkeleton on 2020-02-01 12:57_

---

_Label `P2: need to have` added by @CreepySkeleton on 2020-02-01 12:57_

---

_Label `T: bug` added by @CreepySkeleton on 2020-02-01 12:57_

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-02-01 12:57_

---

_Label `C: asserts` added by @pksunkara on 2020-04-09 08:08_

---

_Comment by @pksunkara on 2020-04-09 08:09_

**NOTE**: `.help_short()` has been changed to `.mut_arg()`

```rust
.mut_arg("help", |h| {
    h.short('H').long("help").help("Print help information")
})
```

---

_Closed by @bors[bot] on 2020-04-10 02:00_

---
