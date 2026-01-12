```yaml
number: 3434
title: "fix: method `red` not found in release builds"
type: pull_request
state: merged
author: MichaReiser
labels:
  - release
assignees: []
merged: true
base: main
head: fix/method-red-not-found-release-build
created_at: 2023-03-10T08:48:27Z
updated_at: 2023-03-10T09:17:36Z
url: https://github.com/astral-sh/ruff/pull/3434
synced_at: 2026-01-12T15:55:12Z
```

# fix: method `red` not found in release builds

---

_@MichaReiser_

Release builds are currently failing with

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

This PR adds the missing `colored::Colorize` import to the release build only panic hook handler in the CLI.

Fixes #3432

---

_Label `release` added by @MichaReiser on 2023-03-10 08:48_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-03-10 08:52_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-03-10 08:52_

---

_Review requested from @konstin by @MichaReiser on 2023-03-10 08:52_

---

_@konstin approved on 2023-03-10 08:56_

---

_Merged by @MichaReiser on 2023-03-10 09:17_

---

_Closed by @MichaReiser on 2023-03-10 09:17_

---

_Branch deleted on 2023-03-10 09:17_

---
