---
number: 1454
title: rustfmt.toml unknown configuration options
type: issue
state: closed
author: omarabid
labels: []
assignees: []
created_at: 2019-04-19T14:22:50Z
updated_at: 2019-06-20T09:18:10Z
url: https://github.com/clap-rs/clap/issues/1454
synced_at: 2026-01-10T01:26:54Z
---

# rustfmt.toml unknown configuration options

---

_Issue opened by @omarabid on 2019-04-19 14:22_

Apparently, these options were available on previous Rust versions and now removed? If that's the case, I'll make the appropriate PR!

### Rust Version

rustc 1.35.0-nightly (96d700f1b 2019-04-10)

### Affected Version of clap

2.31.2

### Bug or Feature Request Summary

Warning: Unknown configuration option `chain_overflow_last`
Warning: Unknown configuration option `same_line_if_else`

---

_Referenced in [clap-rs/clap#1493](../../clap-rs/clap/pulls/1493.md) on 2019-06-19 01:47_

---

_Comment by @erickt on 2019-06-19 16:46_

This has been fixed in the v3-master branch, and can probably be closed.

---

_Closed by @omarabid on 2019-06-20 09:18_

---
