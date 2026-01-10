```yaml
number: 19145
title: Consider removing deprecated macOS config file discovery
type: issue
state: closed
author: ntBre
labels:
  - configuration
assignees: []
created_at: 2025-07-04T14:57:47Z
updated_at: 2025-09-05T14:24:20Z
url: https://github.com/astral-sh/ruff/issues/19145
synced_at: 2026-01-10T11:09:59Z
```

# Consider removing deprecated macOS config file discovery

---

_Issue opened by @ntBre on 2025-07-04 14:57_

In https://github.com/astral-sh/ruff/pull/11115 we moved from defaulting to `$HOME/Library/Application Support` to `$XDG_HOME` on macOS and added a deprecation warning if we find files in the old location:

https://github.com/astral-sh/ruff/blob/411cccb35eafbf91787d36ca6092101328211e2c/crates/ruff_workspace/src/pyproject.rs#L130-L135

This has been deprecated for over a year and since v0.5, so I think we can consider removing it in a future release.

---

_Added to milestone `v0.13` by @ntBre on 2025-07-04 14:57_

---

_Label `configuration` added by @ntBre on 2025-07-04 14:57_

---

_Closed by @ntBre on 2025-09-05 14:24_

---
