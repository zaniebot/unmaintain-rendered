```yaml
number: 6176
title: "[rule proposal] prevent `all` on a len-1 iterable"
type: issue
state: open
author: dorschw
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-07-30T08:46:40Z
updated_at: 2023-08-02T21:32:04Z
url: https://github.com/astral-sh/ruff/issues/6176
synced_at: 2026-01-10T11:09:48Z
```

# [rule proposal] prevent `all` on a len-1 iterable

---

_Issue opened by @dorschw on 2023-07-30 08:46_

```diff
- all([something])
+ something
```

```diff
- all({something})
+ something
```

```diff
- all((something,))
+ something
```

I'd suggest it to flake8-simplify for reusing the error code, but their repo seems inactive for a few months. 
If you have a suggestion for another Python-based linter, I can implement & contribute it there. 

P.S. same goes for `any`

---

_Label `rule` added by @charliermarsh on 2023-08-02 21:32_

---

_Label `needs-decision` added by @charliermarsh on 2023-08-02 21:32_

---
