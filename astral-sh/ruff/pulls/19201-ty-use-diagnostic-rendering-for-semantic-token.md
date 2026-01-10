```yaml
number: 19201
title: "[ty] Use diagnostic rendering for semantic token tests"
type: pull_request
state: closed
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
base: main
head: micha/semantic-token-tests
created_at: 2025-07-08T11:12:15Z
updated_at: 2025-07-09T06:08:50Z
url: https://github.com/astral-sh/ruff/pull/19201
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Use diagnostic rendering for semantic token tests

---

_Pull request opened by @MichaReiser on 2025-07-08 11:12_

## Summary

This PR changes the test infrastructure for semantic tokens to use the diagnostic rendering over printing the tokens with their text range.

The advantage of this approach is that I don't need a hex viewer to verify that the byte offsets are correct. 

However, it does come at the cost of much larger snapshots. I still think it's worth it considering that it improves readability of tests but it's certainly a trade off.

## Test Plan

Updated tests


---

_Comment by @github-actions[bot] on 2025-07-08 11:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review requested from @UnboundVariable by @MichaReiser on 2025-07-08 11:23_

---

_Label `testing` added by @MichaReiser on 2025-07-08 11:24_

---

_Label `ty` added by @MichaReiser on 2025-07-08 11:24_

---

_Marked ready for review by @MichaReiser on 2025-07-08 11:28_

---

_Review requested from @carljm by @MichaReiser on 2025-07-08 11:28_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-08 11:28_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-08 11:28_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-08 11:28_

---

_@AlexWaygood approved on 2025-07-08 11:35_

this is awesome!

---

_Comment by @UnboundVariable on 2025-07-08 15:22_

I don't have a strong opinion. I personally find this somewhat less readable than directly printing the token information in the snapshot, but I think either approach works.

---

_Closed by @MichaReiser on 2025-07-09 06:08_

---
