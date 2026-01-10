---
number: 1358
title: Unclear docs for is_present and value_of
type: issue
state: closed
author: mark-summerfield
labels:
  - A-docs
  - E-help-wanted
  - E-easy
assignees: []
created_at: 2018-10-10T10:01:14Z
updated_at: 2020-12-23T17:45:26Z
url: https://github.com/clap-rs/clap/issues/1358
synced_at: 2026-01-10T01:26:49Z
---

# Unclear docs for is_present and value_of

---

_Issue opened by @mark-summerfield on 2018-10-10 10:01_

### Rust Version

1.29.1

### Affected Version of clap

2.32.0

### Bug or Feature Request Summary

If you have set a default_value then is_present() always returns true and value_of() always returns Some(value) *whether or not* the argument is specified. I think the docs for these two functions should include a note explaining this behavior, and referring users to occurrences_of() if they want to check if the argument was actually specified or not.

---

_Label `C: docs` added by @CreepySkeleton on 2020-02-01 16:00_

---

_Label `good first issue` added by @CreepySkeleton on 2020-02-01 16:00_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-01 16:00_

---

_Added to milestone `v3.0.0-alpha.3` by @CreepySkeleton on 2020-02-01 16:00_

---

_Referenced in [TeXitoi/structopt#368](../../TeXitoi/structopt/issues/368.md) on 2020-03-31 14:18_

---

_Removed from milestone `3.0.0-beta.2` by @pksunkara on 2020-04-07 13:59_

---

_Added to milestone `3.0` by @pksunkara on 2020-04-07 13:59_

---

_Referenced in [clap-rs/clap#2265](../../clap-rs/clap/pulls/2265.md) on 2020-12-23 13:50_

---

_Closed by @pksunkara on 2020-12-23 17:45_

---
