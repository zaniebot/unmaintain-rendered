```yaml
number: 21974
title: "Fix `\\n` in logging's lazy fix for rule G004"
type: pull_request
state: closed
author: richardhapb
labels: []
assignees: []
base: main
head: main
created_at: 2025-12-14T04:46:05Z
updated_at: 2025-12-15T14:19:47Z
url: https://github.com/astral-sh/ruff/pull/21974
synced_at: 2026-01-12T15:57:38Z
```

# Fix `\n` in logging's lazy fix for rule G004

---

_@richardhapb_

## Summary

Fixes #21973. When the fix is applied and a `\n` is present, the transformation introduces a newline and results in a syntax error.

The fix replaces `\n` with `\\n` to preserve correct string rendering.

This change also adds support for `Expr::Call` and `Expr::Attribute` in the fix by replacing the full text range.

The snapshot updates reflect the expanded fix applicability ([*]) and the corrected string rendering. Additionally, a test case with `\n` was added to ensure that no unintended newline is introduced.


---

_Renamed from "Fix `\n` in logging's lazy fix in rule G004" to "Fix `\n` in logging's lazy fix for rule G004" by @richardhapb on 2025-12-14 04:46_

---

_Marked ready for review by @richardhapb on 2025-12-14 04:53_

---

_Comment by @ntBre on 2025-12-15 14:19_

Thanks for your work on this! 

It looks like https://github.com/astral-sh/ruff/issues/21973 hadn't been triaged yet and is actually a duplicate of an older issue (#20151), which already has two open PRs trying to fix it (#20224 and #21310). I'll close this for now to avoid more duplicate work.

---

_Closed by @ntBre on 2025-12-15 14:19_

---
