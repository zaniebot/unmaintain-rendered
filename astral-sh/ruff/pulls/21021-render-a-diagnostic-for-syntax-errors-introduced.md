```yaml
number: 21021
title: Render a diagnostic for syntax errors introduced in formatter tests
type: pull_request
state: merged
author: ntBre
labels:
  - formatter
  - testing
assignees: []
merged: true
base: main
head: brent/format-twice-diagnostic
created_at: 2025-10-21T16:33:12Z
updated_at: 2025-10-21T17:47:28Z
url: https://github.com/astral-sh/ruff/pull/21021
synced_at: 2026-01-10T17:34:34Z
```

# Render a diagnostic for syntax errors introduced in formatter tests

---

_Pull request opened by @ntBre on 2025-10-21 16:33_

## Summary

I spun this out from #21005 because I thought it might be helpful separately. It just renders a nice `Diagnostic` for syntax errors pointing to the source of the error. This seemed a bit more helpful to me than just the byte offset when working on #21005, and we had most of the code around after #20443 anyway.

## Test Plan

This doesn't actually affect any passing tests, but here's an example of the additional output I got when I broke the spacing after the `in` token:

```
    error[internal-error]: Expected 'in', found name
      --> /home/brent/astral/ruff/crates/ruff_python_formatter/resources/test/fixtures/black/cases/cantfit.py:50:79
       |
    48 |     need_more_to_make_the_line_long_enough,
    49 | )
    50 | del ([], name_1, name_2), [(), [], name_4, name_3], name_1[[name_2 for name_1 inname_0]]
       |                                                                               ^^^^^^^^
    51 | del ()
       |
```

I just appended this to the other existing output for now.


---

_Label `formatter` added by @ntBre on 2025-10-21 16:33_

---

_Label `testing` added by @ntBre on 2025-10-21 16:33_

---

_Comment by @github-actions[bot] on 2025-10-21 16:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @ntBre on 2025-10-21 16:44_

---

_Review requested from @MichaReiser by @ntBre on 2025-10-21 16:44_

---

_@MichaReiser approved on 2025-10-21 17:09_

> just the byte offset when working on https://github.com/astral-sh/ruff/pull/21005, and we had most of the code around after https://github.com/astral-sh/ruff/pull/20443 anyway.

Nice! Haha, I installed a hex viewer just for formatter errors

---

_Merged by @ntBre on 2025-10-21 17:47_

---

_Closed by @ntBre on 2025-10-21 17:47_

---

_Branch deleted on 2025-10-21 17:47_

---
