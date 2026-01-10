```yaml
number: 4840
title: "`BestFitting` export ambiguous in `rust_formatter` crate"
type: issue
state: closed
author: zanieb
labels: []
assignees: []
created_at: 2023-06-03T21:24:42Z
updated_at: 2023-06-03T22:13:52Z
url: https://github.com/astral-sh/ruff/issues/4840
synced_at: 2026-01-10T11:09:47Z
```

# `BestFitting` export ambiguous in `rust_formatter` crate

---

_Issue opened by @zanieb on 2023-06-03 21:24_

A run of `cargo +nightly check` warns on this ambiguous export

```
 --> crates/ruff_formatter/src/prelude.rs:1:9
  |
1 | pub use crate::builders::*;
  |         ^^^^^^^^^^^^^^^^^^ the name `BestFitting` in the type namespace is first re-exported here
...
4 | pub use crate::format_element::*;
  |         ------------------------ but the name `BestFitting` in the type namespace is also re-exported here
  |
  = note: `#[warn(ambiguous_glob_reexports)]` on by default
```

https://github.com/charliermarsh/ruff/blob/86ced3516ba8b86581677bd11e89279dfc405a56/crates/ruff_formatter/src/builders.rs#L2134-L2136

https://github.com/charliermarsh/ruff/blob/86ced3516ba8b86581677bd11e89279dfc405a56/crates/ruff_formatter/src/format_element.rs#L292-L297

---

_Comment by @MichaReiser on 2023-06-03 21:30_

Thanks for reporting. I think the best here is to rename the BestFitting in builders to FormatBestFitting

---

_Closed by @MichaReiser on 2023-06-03 22:13_

---
