```yaml
number: 3432
title: Cannot build ruff due erorr in ruff_cli
type: issue
state: closed
author: qarmin
labels: []
assignees: []
created_at: 2023-03-10T07:30:01Z
updated_at: 2023-03-10T09:17:36Z
url: https://github.com/astral-sh/ruff/issues/3432
synced_at: 2026-01-10T11:09:46Z
```

# Cannot build ruff due erorr in ruff_cli

---

_Issue opened by @qarmin on 2023-03-10 07:30_

Main branch  08ec11a3, compilation worked fine with 889c05c8
Ubuntu 22.10, Rust 1.68.0

```
cargo build --release
```

(cargo check and debug cargo build works fine)

```
   Compiling ruff_cli v0.0.254 (/home/rafal/test/ruff/crates/ruff_cli)
error[E0599]: no method named `red` found for reference `&'static str` in the current scope
  --> crates/ruff_cli/src/lib.rs:64:29
   |
64 |                     "error".red().bold(),
   |                             ^^^ method not found in `&'static str`
   |
   = help: items from traits can only be used if the trait is in scope
help: the following trait is implemented but not in scope; perhaps add a `use` for it:
   |
1  | use colored::Colorize;
   |

For more information about this error, try `rustc --explain E0599`.
error: could not compile `ruff_cli` due to previous error
```

Bisect shows
```
a3de791f0af0c0860ea71c994332a6845e9dd6ff is the first bad commit
commit a3de791f0af0c0860ea71c994332a6845e9dd6ff
Author: Micha Reiser <micha@reiser.io>
Date:   Wed Mar 8 12:11:55 2023 +0100

    Make `ruff_cli` binary a small wrapper around `lib` (#3398)

 crates/ruff_cli/Cargo.toml                  |   2 -
 crates/ruff_cli/src/args.rs                 |   2 -
 crates/ruff_cli/src/bin/ruff.rs             |  64 +++++
 crates/ruff_cli/src/commands/config.rs      |   3 +-
 crates/ruff_cli/src/lib.rs                  | 299 ++++++++++++++++++++++--
 crates/ruff_cli/src/main.rs                 | 346 ----------------------------
 crates/ruff_dev/src/generate_all.rs         |  45 ++--
 crates/ruff_dev/src/generate_cli_help.rs    | 103 +++++----
 crates/ruff_dev/src/generate_json_schema.rs |  44 ++--
 9 files changed, 456 insertions(+), 452 deletions(-)
 create mode 100644 crates/ruff_cli/src/bin/ruff.rs
 delete mode 100644 crates/ruff_cli/src/main.rs
```

---

_Comment by @MichaReiser on 2023-03-10 08:48_

Sorry for this. #3434 adds the missing use statement. 

---

_Assigned to @MichaReiser by @MichaReiser on 2023-03-10 08:52_

---

_Closed by @MichaReiser on 2023-03-10 09:17_

---
