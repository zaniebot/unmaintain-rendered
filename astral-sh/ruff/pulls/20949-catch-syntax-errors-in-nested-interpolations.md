```yaml
number: 20949
title: Catch syntax errors in nested interpolations before Python 3.12
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: brent/fix-nested-quotes-again
created_at: 2025-10-17T19:26:40Z
updated_at: 2025-10-20T13:03:17Z
url: https://github.com/astral-sh/ruff/pull/20949
synced_at: 2026-01-10T17:34:34Z
```

# Catch syntax errors in nested interpolations before Python 3.12

---

_Pull request opened by @ntBre on 2025-10-17 19:26_

Summary
--

This PR fixes the issue I added in #20867 and noticed in #20930. Cases like this
cause an error on any Python version:

```py
f"{1:""}"
```

which gave me a false sense of security before. Cases like this are still
invalid only before 3.12 and weren't flagged after the changes in #20867:

```py
f'{1: abcd "{'aa'}" }'
           # ^  reused quote
f'{1: abcd "{"\n"}" }'
            # ^  backslash
```

I didn't recognize these as nested interpolations that also need to be checked
for invalid expressions, so filtering out the whole format spec wasn't quite
right. And `elements.interpolations()` only iterates over the outermost 
interpolations, not the nested ones.

There's basically no code change in this PR, I just moved the existing check
from `parse_interpolated_string`, which parses the entire string, to
`parse_interpolated_element`. This kind of seems more natural anyway and avoids
having to try to recursively visit nested elements after the fact in
`parse_interpolated_string`. So viewing the diff with something like

```
git diff --color-moved --ignore-space-change --color-moved-ws=allow-indentation-change main
```

should make this more clear.

Test Plan
--

New tests

---

_Label `bug` added by @ntBre on 2025-10-17 19:27_

---

_Label `parser` added by @ntBre on 2025-10-17 19:27_

---

_Comment by @github-actions[bot] on 2025-10-17 19:43_

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

_Marked ready for review by @ntBre on 2025-10-17 19:55_

---

_Review requested from @MichaReiser by @ntBre on 2025-10-17 19:55_

---

_Review requested from @dhruvmanila by @ntBre on 2025-10-17 19:55_

---

_@MichaReiser approved on 2025-10-20 07:14_

---

_Merged by @ntBre on 2025-10-20 13:03_

---

_Closed by @ntBre on 2025-10-20 13:03_

---

_Branch deleted on 2025-10-20 13:03_

---
