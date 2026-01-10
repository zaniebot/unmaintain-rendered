```yaml
number: 20411
title: Unfork annotate-snippets
type: issue
state: open
author: MichaReiser
labels:
  - dependencies
assignees: []
created_at: 2025-09-15T07:51:57Z
updated_at: 2025-10-27T20:18:18Z
url: https://github.com/astral-sh/ruff/issues/20411
synced_at: 2026-01-10T11:09:59Z
```

# Unfork annotate-snippets

---

_Issue opened by @MichaReiser on 2025-09-15 07:51_

The initial reason for forking annotate-snippets was that it didn't support [omitting a header](https://github.com/rust-lang/annotate-snippets-rs/issues/167). This seems to be supported with 0.12 or newer. 

However, we've since made new changes. We should explore if we can migrate back to the upstream version.   

---

_Label `dependencies` added by @MichaReiser on 2025-09-15 07:51_

---

_Comment by @pajod on 2025-10-27 20:18_

Using 0.12.0 or above might also fix `␀` rendering in console output.. at least [changelog](https://github.com/rust-lang/annotate-snippets-rs/blob/master/CHANGELOG.md#0120---2025-08-28) promises "Various rendering fixes".

---
