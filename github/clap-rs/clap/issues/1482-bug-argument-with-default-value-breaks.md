---
number: 1482
title: "Bug: argument with default value breaks ArgRequiredElseHelp"
type: issue
state: closed
author: ghost
labels: []
assignees: []
created_at: 2019-05-31T23:12:47Z
updated_at: 2020-02-01T19:55:11Z
url: https://github.com/clap-rs/clap/issues/1482
synced_at: 2026-01-07T13:12:19-06:00
---

# Bug: argument with default value breaks ArgRequiredElseHelp

---

_Issue opened by @ghost on 2019-05-31 23:12_

### Rust Version
rustc 1.37.0-nightly (3ade426ed 2019-05-30)

### Affected Version of clap
2.33.0

### Bug or Feature Request Summary
If you have an argument with a default value, the help message will no longer be printed if you call the application with no arguments, even if `.setting(AppSettings::ArgRequiredElseHelp)` has been enabled.

---

_Comment by @CreepySkeleton on 2020-02-01 13:32_

Yes, this is intended behavior. `default_value` has priority over `.setting(ArgRequiredElseHelp)`. Feel free to reopen

---

_Closed by @CreepySkeleton on 2020-02-01 13:32_

---

_Label `T: bug` added by @pksunkara on 2020-02-01 19:55_

---

_Label `W: wont do` added by @pksunkara on 2020-02-01 19:55_

---

_Label `T: bug` removed by @pksunkara on 2020-02-01 19:55_

---
