```yaml
number: 3107
title: Add line highlighting support for matching lines
type: pull_request
state: closed
author: emrebengue
labels:
  - rollup
assignees: []
base: master
head: master
created_at: 2025-07-24T02:24:48Z
updated_at: 2025-09-20T01:08:46Z
url: https://github.com/BurntSushi/ripgrep/pull/3107
synced_at: 2026-01-12T18:23:15Z
```

# Add line highlighting support for matching lines

---

_@emrebengue_

Implement a new "highlight" feature for the '--colors' flag that allows users to highlight entire matching lines with background color.

 It is important to note that this feature gets disabled if '--colors' flag is used with 'match:none' as it depends on matching detection.

Documentation in crates/core/flags/defs.rs is updated.

Fixes #3024

---

_@BurntSushi reviewed on 2025-08-20 01:18_

---

_Review comment by @BurntSushi on `crates/printer/src/standard.rs`:1467 on 2025-08-20 01:18_

These sorts of unrelated changes are really annoying IMO.

---

_@BurntSushi reviewed on 2025-08-20 01:29_

---

_Review comment by @BurntSushi on `crates/printer/src/standard.rs`:1549 on 2025-08-20 01:29_

This is FUBAR. In `match_spec_with_highlight`, you're doing shenanigans based on whether specific stylings are set. That's no good and it assumes too much about how the user's _intention_ when using `highlight`. If the user wants the background color to be consistent, for example, then they'll need to set it for both the `matched` and `highlight` types.

---

_Label `rollup` added by @BurntSushi on 2025-08-20 01:42_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
