```yaml
number: 1589
title: "Structopt-like example from README doesn't compile"
type: issue
state: closed
author: ghost
labels:
  - C-bug
  - A-docs
  - A-derive
assignees: []
created_at: 2019-10-30T09:25:22Z
updated_at: 2019-10-31T01:54:38Z
url: https://github.com/clap-rs/clap/issues/1589
synced_at: 2026-01-12T16:14:11Z
```

# Structopt-like example from README doesn't compile

---

_@ghost_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

* 1.38

### Affected Version of clap

3.0.0-beta.1

### Expected Behavior Summary

The structopt-like example provided in the README doesn't compile. Errors:

```
error: expected one of `::`, `=>`, `if`, or `|`, found `@`
  --> src/main.rs:54:26
   |
54 |         SubCommand::Test @ t => {
   |                          ^ expected one of `::`, `=>`, `if`, or `|` here

error: proc-macro derive panicked
  --> src/main.rs:10:10
   |
10 | #[derive(Clap)]
   |          ^^^^
   |
   = help: message: unsupported option: parse_from_occurrences
error: aborting due to 2 previous errors
```

In my Cargo.toml I have:

```
[dependencies]
clap = { git = "https://github.com/clap-rs/clap", features = ["wrap_help"] }
```

---

_Comment by @kbknapp on 2019-10-30 21:07_

Thanks for the report :+1:  We'll get it fixed and push an update.

---

_Label `C: derive macros` added by @kbknapp on 2019-10-30 21:09_

---

_Label `C: docs` added by @kbknapp on 2019-10-30 21:09_

---

_Label `C: examples` added by @kbknapp on 2019-10-30 21:09_

---

_Label `D: easy` added by @kbknapp on 2019-10-30 21:09_

---

_Label `P2: need to have` added by @kbknapp on 2019-10-30 21:09_

---

_Label `T: bug` added by @kbknapp on 2019-10-30 21:09_

---

_Referenced in [clap-rs/clap#1591](../../clap-rs/clap/pulls/1591.md) on 2019-10-31 00:27_

---

_Closed by @kbknapp on 2019-10-31 01:54_

---
