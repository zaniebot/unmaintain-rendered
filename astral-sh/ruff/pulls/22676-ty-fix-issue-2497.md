```yaml
number: 22676
title: "[ty] Fix issue 2497"
type: pull_request
state: open
author: MichaReiser
labels:
  - bug
  - server
  - ty
assignees: []
draft: true
base: main
head: claude/fix-issue-2497-j5GU2
created_at: 2026-01-18T11:58:49Z
updated_at: 2026-01-18T12:32:51Z
url: https://github.com/astral-sh/ruff/pull/22676
synced_at: 2026-01-18T13:20:54Z
```

# [ty] Fix issue 2497

---

_@MichaReiser_

## Summary

When a doctest ended on a blank line, `in_doctest` was not reset to `false`. This could cause issues when a docstring contained a doctest followed by a literal block (::) with blank lines inside it.

In that scenario, a blank line inside the literal block would incorrectly trigger the doctest-ending logic (because `in_doctest` was still `true` from the earlier doctest), prematurely closing the literal block and causing subsequent code content to be rendered as regular text with `&nbsp;` indentation instead of being inside the code block.

Fixes https://github.com/astral-sh/ty/issues/2497

## Test Plan

Added test

---

_Marked ready for review by @MichaReiser on 2026-01-18 12:32_

---

_Review requested from @carljm by @MichaReiser on 2026-01-18 12:32_

---

_Review requested from @AlexWaygood by @MichaReiser on 2026-01-18 12:32_

---

_Review requested from @sharkdp by @MichaReiser on 2026-01-18 12:32_

---

_Review requested from @dcreager by @MichaReiser on 2026-01-18 12:32_

---

_Review requested from @Gankra by @MichaReiser on 2026-01-18 12:32_

---

_Converted to draft by @MichaReiser on 2026-01-18 12:32_

---

_Label `bug` added by @MichaReiser on 2026-01-18 12:32_

---

_Label `server` added by @MichaReiser on 2026-01-18 12:32_

---

_Label `ty` added by @MichaReiser on 2026-01-18 12:32_

---

_Renamed from "Fix issue 2497" to "[ty] Fix issue 2497" by @MichaReiser on 2026-01-18 12:32_

---
