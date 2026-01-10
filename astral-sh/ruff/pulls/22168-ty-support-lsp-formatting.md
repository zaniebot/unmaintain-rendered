```yaml
number: 22168
title: "[ty] support lsp formatting"
type: pull_request
state: closed
author: Decodetalkers
labels:
  - ty
assignees: []
base: main
head: support_format
created_at: 2025-12-24T03:07:03Z
updated_at: 2025-12-24T09:25:26Z
url: https://github.com/astral-sh/ruff/pull/22168
synced_at: 2026-01-10T16:36:18Z
```

# [ty] support lsp formatting

---

_Pull request opened by @Decodetalkers on 2025-12-24 03:07_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This pr is for lsp formatting support

This pr do not support range format, it format the whole content.


### Before
<img width="1002" height="663" alt="image" src="https://github.com/user-attachments/assets/e063ebee-7250-46dc-abc4-f585dfaafd98" />

### After
<img width="741" height="583" alt="image" src="https://github.com/user-attachments/assets/1b92d2c1-7bbf-434b-be1c-99ba9aa2b75a" />


<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

run the lsp and try do format by using lsp to format the content

<!-- How was it tested? -->


---

_Review requested from @carljm by @Decodetalkers on 2025-12-24 03:07_

---

_Review requested from @MichaReiser by @Decodetalkers on 2025-12-24 03:07_

---

_Review requested from @sharkdp by @Decodetalkers on 2025-12-24 03:07_

---

_Review requested from @dcreager by @Decodetalkers on 2025-12-24 03:07_

---

_Review requested from @AlexWaygood by @Decodetalkers on 2025-12-24 03:07_

---

_Converted to draft by @Decodetalkers on 2025-12-24 03:07_

---

_Marked ready for review by @Decodetalkers on 2025-12-24 03:31_

---

_Comment by @MichaReiser on 2025-12-24 07:29_

Thank you for looking into this. 

We don't plan on supporting formatting in ty in the near future. Instead, we recommend users Ruff for formatting and linting.

---

_Closed by @MichaReiser on 2025-12-24 07:29_

---

_Label `ty` added by @AlexWaygood on 2025-12-24 09:25_

---

_Renamed from "feat: support lsp formatting" to "[ty] support lsp formatting" by @AlexWaygood on 2025-12-24 09:25_

---
