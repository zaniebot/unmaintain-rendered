```yaml
number: 19868
title: "Request: `PLC0415` autofix"
type: issue
state: open
author: jamesbraza
labels:
  - fixes
  - preview
assignees: []
created_at: 2025-08-11T16:51:45Z
updated_at: 2025-08-14T02:28:07Z
url: https://github.com/astral-sh/ruff/issues/19868
synced_at: 2026-01-12T15:54:57Z
```

# Request: `PLC0415` autofix

---

_@jamesbraza_

### Summary

Without a custom rule set, I observe the AI coding agents commonly employing local imports. It's not uncommon for me to see PRs opened with built-ins (e.g. `import os`) being imported inside a method.

Ruff has `PLC0415` implemented to deny this, but since it doesn't have autofix support, it can get disabled.

The request essentially boils down to: a common form of "AI slop" is non-module level imports. Is it possible to add autofix support to PLC0415 to make this process smoother.

---

_Comment by @ntBre on 2025-08-11 17:44_

Makes sense to me. I think we have good infrastructure for adding imports too, so I'm guessing the fix would look something like:
1. delete the local import
2. add the module-level import

Since the rule is stable we'd need to add the fix in preview, especially if it's a safe fix, which it probably could be?

---

_Label `fixes` added by @ntBre on 2025-08-11 17:44_

---

_Label `preview` added by @ntBre on 2025-08-11 17:44_

---

_Comment by @jamesbraza on 2025-08-11 17:54_

That sounds good. From my perspective, times where a `PLC0415` autofix could be unsafe are:

- Case 1: avoiding circular imports via a lazy import of a third party or local package
    - Imo this warrants an explicit `# noqa: PLC0415`
- Case 2: unit tests with monkeypatching requiring lazy import
    - Imo this also warrants an explicit `# noqa: PLC0415`

So I think the classification would be:

- If built-in: autofix is safe
- If third party or local: autofix is unsafe

---

_Comment by @ntBre on 2025-08-11 19:14_

Ah those are great points. I don't think I was weighing those cases enough earlier. Special-casing built-ins makes some sense to me, or we could even just make it always unsafe.

---

_Comment by @nathanjmcdougall on 2025-08-14 02:27_

Imports can have side effects, even in the standard library, so I think they should always be considered unsafe to move.

For example, `import antigravity` from the stdlib opens the web browser.

---
