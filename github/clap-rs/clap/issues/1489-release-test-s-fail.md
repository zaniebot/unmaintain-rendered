---
number: 1489
title: Release test(s) fail
type: issue
state: closed
author: TyPR124
labels: []
assignees: []
created_at: 2019-06-12T05:02:58Z
updated_at: 2019-06-27T01:03:37Z
url: https://github.com/clap-rs/clap/issues/1489
synced_at: 2026-01-07T13:12:19-06:00
---

# Release test(s) fail

---

_Issue opened by @TyPR124 on 2019-06-12 05:02_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

rustc 1.35.0 (3c235d560 2019-05-20)

### Affected Version of clap

2.33.0 + 784524f (Current master as of writing)

v3-master branch as of  b5af579

### Expected Behavior Summary
Tests to pass

### Actual Behavior Summary
At least one test fails

`test non_existing_arg_in_subcommand_help ... FAILED`

### Steps to Reproduce the issue
cargo test --release


---

_Referenced in [clap-rs/clap#1490](../../clap-rs/clap/pulls/1490.md) on 2019-06-13 00:54_

---

_Referenced in [clap-rs/clap#1511](../../clap-rs/clap/pulls/1511.md) on 2019-06-26 02:28_

---

_Closed by @TyPR124 on 2019-06-27 01:03_

---
