```yaml
number: 16588
title: Remove temporary with_added_extension in favour of stablised standard method
type: pull_request
state: closed
author: crh23
labels: []
assignees: []
draft: true
base: main
head: refactor/with_added_extension
created_at: 2025-11-04T10:47:34Z
updated_at: 2025-11-04T10:52:02Z
url: https://github.com/astral-sh/uv/pull/16588
synced_at: 2026-01-12T16:12:20Z
```

# Remove temporary with_added_extension in favour of stablised standard method

---

_@crh23_

## Summary

Replace the with_added_extension function created in #15300 (and moved in #15610) with the now-stabilised version PathBuf::with_added_extension. 
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Uses existing tests (which were added in the original PR specifically to enable this change)


---

_Converted to draft by @crh23 on 2025-11-04 10:50_

---

_Comment by @crh23 on 2025-11-04 10:52_

Ah scratch that - I'll close this until project rust version reaches 1.91.0

---

_Closed by @crh23 on 2025-11-04 10:52_

---
