---
number: 1614
title: "clap::Error replaces io::Error messages with \"description() is deprecated\""
type: issue
state: closed
author: keturn
labels: []
assignees: []
created_at: 2019-12-29T01:31:34Z
updated_at: 2020-07-07T15:05:13Z
url: https://github.com/clap-rs/clap/issues/1614
synced_at: 2026-01-07T13:12:19-06:00
---

# clap::Error replaces io::Error messages with "description() is deprecated"

---

_Issue opened by @keturn on 2019-12-29 01:31_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

1.40.0

### Affected Version of clap

2.33.0

### Expected Behavior Summary

IO Error is displayed with human-readable description.

### Actual Behavior Summary

```
Error: Error { message: "\u{1b}[1;31merror:\u{1b}[0m description() is deprecated; use Display", kind: Io, info: None }
```

### Sample Code or Link to Sample Code

tests in PR #1615 

---

_Referenced in [clap-rs/clap#1615](../../clap-rs/clap/pulls/1615.md) on 2019-12-29 01:31_

---

_Comment by @keturn on 2019-12-29 01:35_

Error::description looks to have been [deprecated back in 1.27](https://github.com/rust-lang/rust/blob/master/RELEASES.md#compatibility-notes-12), which I think is well below your minimum version requirement even for the v2 branch.

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-02-01 10:52_

---

_Label `C: errors` added by @CreepySkeleton on 2020-02-01 10:52_

---

_Label `D: easy` added by @CreepySkeleton on 2020-02-01 10:52_

---

_Label `P2: need to have` added by @CreepySkeleton on 2020-02-01 10:52_

---

_Label `W: 2.x` added by @CreepySkeleton on 2020-02-01 10:52_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-01 10:52_

---

_Comment by @pksunkara on 2020-02-01 20:59_

This is actually a v2 issue and is probably fixed in v3. Fix might be backporting #1631.

---

_Label `W: 3.x` removed by @CreepySkeleton on 2020-02-02 02:36_

---

_Removed from milestone `3.0` by @CreepySkeleton on 2020-02-02 02:36_

---

_Comment by @CreepySkeleton on 2020-07-07 15:05_

This is fixed in master because the only way to get `description() is deprecated; use Display` is to explicitly call `description()`, and my grep-fu suggests we don't do that. This is fixed in master, and we don't plan on backporting it to 2.x.

---

_Closed by @CreepySkeleton on 2020-07-07 15:05_

---
