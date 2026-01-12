```yaml
number: 20660
title: "[`pyupgrade`] Fix false negative for `TypeVar` with default argument in `non-pep695-generic-class` (`UP046`)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-20656
created_at: 2025-10-01T03:49:46Z
updated_at: 2025-10-15T14:58:20Z
url: https://github.com/astral-sh/ruff/pull/20660
synced_at: 2026-01-12T15:57:07Z
```

# [`pyupgrade`] Fix false negative for `TypeVar` with default argument in `non-pep695-generic-class` (`UP046`)

---

_@danparizher_

## Summary

Fixes #20656


---

_Comment by @github-actions[bot] on 2025-10-01 03:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:183 on 2025-10-09 20:42_

tiny nit: Is this shared by all the match arms? If so we could factor it out into a shared `default` variable. I also think we can do without the explicit deref but maybe clippy complains.


```suggestion
                default: default.map(|expr| Box::new(expr.clone())),
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:387 on 2025-10-09 20:43_

We need to preview gate this as I mentioned on the issue. I think this might be the easiest place to do that, assuming every usage goes through this code path.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP047.py.snap`:1 on 2025-10-09 20:43_

As demonstrated here, this change also affects UP047. I think it should also affect UP040, so we should make sure we have test coverage for that as well.

---

_@ntBre reviewed on 2025-10-09 20:47_

Thank you, this is looking good. I guess I expected this to be more complicated to have left the TODO around for so long 😆 I thought we would need an explicit Python 3.13 check somewhere, but it looks like the `default` argument to the `typing.TypeVar` constructors was only added in 3.13 too. So it seems safe to assume we're on 3.13+ if we find one.

As I noted in the inline comments, I think we need to make this a preview change and also check on UP040 test cases. I think all three of UP040, UP046, and UP047 share this same infrastructure.

---

_Label `rule` added by @ntBre on 2025-10-09 20:48_

---

_Label `preview` added by @ntBre on 2025-10-09 20:48_

---

_@ntBre reviewed on 2025-10-13 20:43_

The code changes look good, but now that this is a preview change, we also need to run the  test cases in preview mode.

---

_Review requested from @ntBre by @danparizher on 2025-10-15 00:55_

---

_@ntBre approved on 2025-10-15 14:48_

Thanks!

I just pushed a few commits adding more of the test cases to the preview tests and using  the new `assert_diagnostics_diff` macro. I also cleaned  up some of the resolved  TODOs in the tests.

---

_Merged by @ntBre on 2025-10-15 14:51_

---

_Closed by @ntBre on 2025-10-15 14:51_

---

_Branch deleted on 2025-10-15 14:57_

---
