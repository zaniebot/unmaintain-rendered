```yaml
number: 20529
title: "Standardize traverse union behavior in `TypeChecker`s."
type: issue
state: open
author: PieterCK
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-09-23T06:22:13Z
updated_at: 2025-09-23T12:29:16Z
url: https://github.com/astral-sh/ruff/issues/20529
synced_at: 2026-01-10T11:09:59Z
```

# Standardize traverse union behavior in `TypeChecker`s.

---

_Issue opened by @PieterCK on 2025-09-23 06:22_

Some `TypeChecker`s `match_annotation` implementation doesn't traverse unions, while others do.

[Comment](https://github.com/astral-sh/ruff/pull/20264#discussion_r2369810010) from @ntBre:
> ... I feel like we should probably either always traverse unions or never traverse unions and apply that decision to all of our TypeCheckers rather than keep a mix of them.

---

_Comment by @ntBre on 2025-09-23 12:28_

Thanks for opening the issue!

I don't feel too strongly here either way, and I haven't vetted all the uses of all the `TypeChecker`s to be sure if this would work, but at a high level it makes sense to me to be consistent. I can also see an argument either way, to always do this or never do it, but lean towards always traversing unions myself. I'm interested to hear other thoughts!

---

_Label `rule` added by @ntBre on 2025-09-23 12:28_

---

_Label `needs-decision` added by @ntBre on 2025-09-23 12:28_

---
