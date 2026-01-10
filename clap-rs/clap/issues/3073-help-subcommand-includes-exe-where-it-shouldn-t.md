---
number: 3073
title: "Help subcommand includes `.exe` where it shouldn't"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2021-12-07T21:51:48Z
updated_at: 2021-12-08T00:30:29Z
url: https://github.com/clap-rs/clap/issues/3073
synced_at: 2026-01-10T01:27:31Z
---

# Help subcommand includes `.exe` where it shouldn't

---

_Issue opened by @epage on 2021-12-07 21:51_

`08_subcommands[.exe-add` doesn't looks correct from `examples/08_subcommands.rs`:
```
$ 08_subcommands help add
08_subcommands[.exe-add 0.1

Kevin K.

Adds files to myapp

USAGE:
    08_subcommands.exe add <input>

ARGS:
    <input>    the file to add

OPTIONS:
    -h, --help       Print help information
    -V, --version    Print version information
```

---

_Label `T: bug` added by @epage on 2021-12-07 21:51_

---

_Label `C: help message` added by @epage on 2021-12-07 21:51_

---

_Comment by @pksunkara on 2021-12-08 00:16_

Duplicate of #992?

---

_Comment by @epage on 2021-12-08 00:30_

Good catch

---

_Closed by @epage on 2021-12-08 00:30_

---
