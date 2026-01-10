---
number: 1871
title: "Regression: version option not working on master"
type: issue
state: closed
author: pksunkara
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2020-04-27T13:19:29Z
updated_at: 2020-04-27T19:37:53Z
url: https://github.com/clap-rs/clap/issues/1871
synced_at: 2026-01-10T01:27:08Z
---

# Regression: version option not working on master

---

_Issue opened by @pksunkara on 2020-04-27 13:19_

### Steps to reproduce the issue

```
cargo run --example 09_auto_version -- --version
```

### Actual Behavior Summary

No output

### Expected Behavior Summary

Version to be printed

### Additional context

Definitely related to the termcolor refactor

---

_Label `T: bug` added by @pksunkara on 2020-04-27 13:19_

---

_Added to milestone `3.0` by @pksunkara on 2020-04-27 13:19_

---

_Label `T: regression` added by @CreepySkeleton on 2020-04-27 13:42_

---

_Label `C: help message` added by @CreepySkeleton on 2020-04-27 13:42_

---

_Comment by @CreepySkeleton on 2020-04-27 13:47_

Likely has something to do with `exit(0)`. We need `.flush()` somewhere.

---

_Referenced in [clap-rs/clap#1206](../../clap-rs/clap/issues/1206.md) on 2020-04-27 17:02_

---

_Referenced in [clap-rs/clap#1873](../../clap-rs/clap/pulls/1873.md) on 2020-04-27 18:52_

---

_Closed by @bors[bot] on 2020-04-27 19:37_

---

_Referenced in [epage/clapng#90](../../epage/clapng/issues/90.md) on 2021-12-06 16:41_

---

_Referenced in [clap-rs/clap#5542](../../clap-rs/clap/pulls/5542.md) on 2024-06-20 04:18_

---
