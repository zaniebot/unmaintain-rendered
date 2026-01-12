```yaml
number: 2875
title: "Remove dependency on `ruff_rowan`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/formatter-iv
created_at: 2023-02-13T23:37:30Z
updated_at: 2023-02-15T03:54:10Z
url: https://github.com/astral-sh/ruff/pull/2875
synced_at: 2026-01-12T04:52:01Z
```

# Remove dependency on `ruff_rowan`

---

_Pull request opened by @charliermarsh on 2023-02-13 23:37_

This PR removes the dependency on `ruff_rowan` (i.e., Rome's fork of rust-analyzer's `rowan`), and in turn, trims out a lot of code in `ruff_formatter` that isn't necessary (or isn't _yet_ necessary) to power the autoformatter.

We may end up pulling some of this back in -- TBD. For example, the autoformatter has its own comment representation right now, but we may eventually want to use the `comments.rs` data structures defined in `rome_formatter`.

---

_Label `autoformatter` added by @charliermarsh on 2023-02-13 23:37_

---

_Comment by @charliermarsh on 2023-02-13 23:37_

\cc @MichaReiser 

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/format_extensions.rs`:71 on 2023-02-14 08:50_

You probably want to keep this around. Memoization is essential to get good formatting performance when using the `BestFitting` IR

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/lib.rs`:229 on 2023-02-14 08:52_

We may need source maps in the long term to enable range formatting (formatting the selected text) or to suppress formatting of specific nodes 

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/token/number.rs`:9 on 2023-02-14 08:54_

We may use some of this (together with `string`) to normalize the string/number content. But we can pull this in when needed.

---

_Review comment by @MichaReiser on `crates/ruff_formatter/Cargo.toml`:11 on 2023-02-14 08:55_

Did you verify if all these dependencies are still needed? https://github.com/est31/cargo-udeps

---

_@MichaReiser reviewed on 2023-02-14 08:55_

LGTM

---

_@charliermarsh reviewed on 2023-02-15 03:37_

---

_Review comment by @charliermarsh on `crates/ruff_formatter/src/token/number.rs`:9 on 2023-02-15 03:37_

Yeah we'll absolutely need this kind of logic. But does it belong here, or in `ruff_python_formatter`? Black has rules around this stuff, but some of it is Python-specific (e.g., for complex numbers).

---

_@charliermarsh reviewed on 2023-02-15 03:42_

---

_Review comment by @charliermarsh on `crates/ruff_formatter/Cargo.toml`:11 on 2023-02-15 03:42_

Was able to remove a handful of them -- good call.

---

_Merged by @charliermarsh on 2023-02-15 03:54_

---

_Closed by @charliermarsh on 2023-02-15 03:54_

---

_Branch deleted on 2023-02-15 03:54_

---
