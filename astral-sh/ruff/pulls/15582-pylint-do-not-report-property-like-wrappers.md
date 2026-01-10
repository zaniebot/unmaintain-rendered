```yaml
number: 15582
title: "[`pylint`] Do not report property-like wrappers (`PLE0302`)"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - rule
  - needs-decision
assignees: []
base: main
head: PLE0302
created_at: 2025-01-19T01:14:06Z
updated_at: 2025-04-28T07:13:59Z
url: https://github.com/astral-sh/ruff/pull/15582
synced_at: 2026-01-10T19:03:00Z
```

# [`pylint`] Do not report property-like wrappers (`PLE0302`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-19 01:14_

## Summary

Resolves #15572.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-19 01:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-01-19 10:05_

Would you mind extending your summary with an explanation why you think we should allow property wrapped context managers. It seems you already did some research and writing it down would save me the work to do the same. 

---

_Comment by @InSyncWithFoo on 2025-01-19 12:54_

@MichaReiser This is not specific to any dunder methods. The issue's example is a bit too complex, so here's a simplification:

```python
class C:
	# This is the normal signature and one expected by PLE0302
	def __getitem__(self, other): ...


def some_function(other): ...

class D:
	@property
	def __getitem__(self):
		return some_function

d = D()
d[1]  # Calls `d.__getitem__(1)`.
      # `d.__getitem__` returns `some_function`,
      # so the eventual result is that of `some_function(1)`.
```

---

_Label `rule` added by @dylwil3 on 2025-01-19 13:29_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-19 18:35_

---

_Comment by @charliermarsh on 2025-01-19 18:36_

I'll fix the docs and merge.

---

_Comment by @charliermarsh on 2025-01-19 18:41_

I asked one question in the linked issue.

---

_Label `needs-decision` added by @MichaReiser on 2025-02-04 09:51_

---

_Comment by @MichaReiser on 2025-04-28 07:13_

Closing, see the referenced issue for why

---

_Closed by @MichaReiser on 2025-04-28 07:13_

---
