```yaml
number: 20381
title: "[`pylint`] Exempt required imports from `PLR0402`"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-20380
created_at: 2025-09-15T00:10:23Z
updated_at: 2025-09-25T21:21:35Z
url: https://github.com/astral-sh/ruff/pull/20381
synced_at: 2026-01-12T15:57:00Z
```

# [`pylint`] Exempt required imports from `PLR0402`

---

_@danparizher_

## Summary

Fixes #20380

The fix changes the `includes_import` function in `I002` to recognize equivalent imports based on bound name and qualified name rather than exact syntax matching.

This allows the function to correctly identify that `from concurrent import futures` satisfies the requirement for `import concurrent.futures as futures`.

---

_Comment by @github-actions[bot] on 2025-09-15 00:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dylwil3 by @dylwil3 on 2025-09-15 13:28_

---

_Comment by @dylwil3 on 2025-09-19 18:12_

Hmmm... I'm a bit conflicted here about whether `I002` or `PLR0402` should take precedence to resolve the infinite loop. Generally it feels like the more "customized" or "manually entered" choice should take precedence. Since a user enters required imports exactly as they'd like them to appear, whereas `PLR0402` is a "general" rule, I'm tempted to say that it's `PLR0402` that should be modified here.

Would it be possible to have `PLR0402` skip if it's triggering on a required import instead? I would also be open to adding either a warning or a debug message explaining why the rule is being skipped in this case. (Probably a debug message since we wouldn't have a good way to override the warning).

What do you think?

---

_Comment by @ntBre on 2025-09-19 18:26_

I agree with Dylan, that seems closer to how we've handled the other `I002` conflicts recently (https://github.com/astral-sh/ruff/pull/20327, https://github.com/astral-sh/ruff/pull/19413). Possibly we can even reuse the helper method you added in #19413 :)

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_future_import.rs`:119 on 2025-09-22 13:13_

Could you compare qualified names directly and use this method? https://github.com/astral-sh/ruff/blob/0c7cfd2a8d60e9e09cfd75c9d521f71a8be0b592/crates/ruff_python_ast/src/name.rs#L247

I believe it's designed to not allocate on the heap when there's a small number of segments.

---

_@dylwil3 requested changes on 2025-09-22 13:16_

Thanks! One nit

---

_Review requested from @dylwil3 by @danparizher on 2025-09-22 19:46_

---

_@dylwil3 approved on 2025-09-25 21:06_

---

_Merged by @dylwil3 on 2025-09-25 21:08_

---

_Closed by @dylwil3 on 2025-09-25 21:08_

---

_Label `bug` added by @dylwil3 on 2025-09-25 21:08_

---

_Label `rule` added by @dylwil3 on 2025-09-25 21:08_

---

_Renamed from "[`isort`] Fix infinite loop when checking equivalent imports (`I002`, `PLR0402`)" to "[`pylint`] Exempt required imports from `PLR0402`" by @dylwil3 on 2025-09-25 21:08_

---

_Branch deleted on 2025-09-25 21:21_

---
