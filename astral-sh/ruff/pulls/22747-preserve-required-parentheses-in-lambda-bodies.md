```yaml
number: 22747
title: Preserve required parentheses in lambda bodies
type: pull_request
state: open
author: ntBre
labels:
  - bug
  - formatter
  - preview
assignees: []
base: main
head: brent/lambda-walrus
created_at: 2026-01-19T21:38:34Z
updated_at: 2026-01-19T22:38:01Z
url: https://github.com/astral-sh/ruff/pull/22747
synced_at: 2026-01-19T23:36:38Z
```

# Preserve required parentheses in lambda bodies

---

_@ntBre_

Summary
--

This PR fixes the issues revealed in #22744 by adding an additional branch to
the lambda body formatting that checks if the body `needs_parentheses` before
falling back on the `Parentheses::Never` case. I also updated the
`ExprNamed::needs_parentheses` implementation to match the one from #8465.

Test Plan
--

New test based on the failing cases in #22744. I also checked out #22744 and
checked that the tests pass after applying the changes from this PR.


---

_Label `bug` added by @ntBre on 2026-01-19 21:38_

---

_Label `formatter` added by @ntBre on 2026-01-19 21:38_

---

_Label `preview` added by @ntBre on 2026-01-19 21:38_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 22:28_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Marked ready for review by @ntBre on 2026-01-19 22:28_

---

_Review requested from @MichaReiser by @ntBre on 2026-01-19 22:28_

---

_Review requested from @dylwil3 by @ntBre on 2026-01-19 22:29_

---

_@dylwil3 approved on 2026-01-19 22:36_

Looks good to me! Strange we missed what appears to be exactly one case from PEP 572 in `needs_parentheses`, but I just checked again and it seems like they're all there now.

---

_Comment by @ntBre on 2026-01-19 22:38_

Oh good idea to double-check that, thank you!

---
