```yaml
number: 1564
title: "clippy::cyclomatic_complexity is deprecated"
type: issue
state: closed
author: alarsyo
labels: []
assignees: []
created_at: 2019-10-04T22:19:25Z
updated_at: 2019-10-21T10:26:42Z
url: https://github.com/clap-rs/clap/issues/1564
synced_at: 2026-01-12T16:14:11Z
```

# clippy::cyclomatic_complexity is deprecated

---

_@alarsyo_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->
### Rust Version

* `rustc 1.38.0 (625451e37 2019-09-23)`

### Affected Version of clap

* `3.0.0-beta.1`

I'm getting a warning about `clippy::cyclomatic_complexity` having been renamed to `clippy::cognitive_complexity` when running clippy on the project:

```
lint `clippy::cyclomatic_complexity` has been renamed to `clippy::cognitive_complexity`
note: `#[warn(renamed_and_removed_lints)]` on by default
help: use the new name: `clippy::cognitive_complexity`rustc(renamed_and_removed_lints)
```




---

_Referenced in [clap-rs/clap#1565](../../clap-rs/clap/pulls/1565.md) on 2019-10-04 22:26_

---

_Closed by @Dylan-DPC-zz on 2019-10-21 10:26_

---
