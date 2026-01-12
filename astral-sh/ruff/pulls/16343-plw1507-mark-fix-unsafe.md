```yaml
number: 16343
title: "[PLW1507] Mark fix unsafe"
type: pull_request
state: merged
author: VascoSch92
labels:
  - fixes
assignees: []
merged: true
base: main
head: PLW1507
created_at: 2025-02-24T08:49:51Z
updated_at: 2025-02-24T12:42:45Z
url: https://github.com/astral-sh/ruff/pull/16343
synced_at: 2026-01-12T15:55:54Z
```

# [PLW1507] Mark fix unsafe

---

_@VascoSch92_

This PR addresses issue #16274 

Just marked the fix as unsafe, as it is not possible to check when a fix is safe or not (see above cited issue for example).

---

_Comment by @github-actions[bot] on 2025-02-24 08:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `fixes` added by @MichaReiser on 2025-02-24 09:14_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pylint/shallow_copy_environ_1.py`:9 on 2025-02-24 09:16_

I think we could keep this test case in the same file but add a comment explaining what it does.

---

_@MichaReiser requested changes on 2025-02-24 09:16_

Thank you

We should add some documentation explaining why the fix is unsafe. You can search for `## Fix safety` to see similar comments for other rules.

---

_Review comment by @VascoSch92 on `crates/ruff_linter/resources/test/fixtures/pylint/shallow_copy_environ_1.py`:9 on 2025-02-24 09:34_

Yes!

---

_@VascoSch92 reviewed on 2025-02-24 09:34_

---

_Comment by @VascoSch92 on 2025-02-24 09:35_

> Thank you
> 
> We should add some documentation explaining why the fix is unsafe. You can search for `## Fix safety` to see similar comments for other rules.

I added the explanation of the unsafe fix. I don't know if it would help to add a small example or/and quoting the issue with the example why the fix is unsafe

---

_Review requested from @MichaReiser by @VascoSch92 on 2025-02-24 09:39_

---

_@MichaReiser approved on 2025-02-24 12:42_

This is perfect, thank you

---

_Merged by @MichaReiser on 2025-02-24 12:42_

---

_Closed by @MichaReiser on 2025-02-24 12:42_

---
