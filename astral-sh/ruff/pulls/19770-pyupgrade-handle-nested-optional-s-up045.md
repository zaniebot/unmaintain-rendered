```yaml
number: 19770
title: "[`pyupgrade`] Handle nested `Optional`s (`UP045`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-19746
created_at: 2025-08-05T19:07:18Z
updated_at: 2025-08-19T22:48:38Z
url: https://github.com/astral-sh/ruff/pull/19770
synced_at: 2026-01-12T15:56:46Z
```

# [`pyupgrade`] Handle nested `Optional`s (`UP045`)

---

_@danparizher_

## Summary

Fixes #19746


---

_Comment by @github-actions[bot] on 2025-08-05 19:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_python_ast/src/helpers.rs`:1499 on 2025-08-06 17:46_

I don't think we need to put this in a `helpers` module, especially not in a different crate, if it's only used in one place.

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/helpers.rs`:1510 on 2025-08-06 17:48_

Isn't this the exact body of `pep_604_optional` where `expr` is `flatten_optional(slice)`?


```suggestion
            pep_604_optional(flattened_inner)
```

at that point, I'd just inline `flattened_inner` too.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep604_annotation.rs`:178 on 2025-08-06 18:02_

I think this needs to be combined with the recursive check in `flatten_optional`. I ran this command on this branch locally, and I believe that this only handles exactly the `Optional[Optional[...]]` case, not arbitrary nesting:

```
cargo run -p ruff --  check --no-cache --select UP045 --diff --target-version py312 - <<<"from typing import Optional;nested_optional: Optional[Optional[Optional[str]]] = None"
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.16s
     Running `target/debug/ruff check --no-cache --select UP045 --diff --target-version py312 -`
@@ -1 +1 @@
-from typing import Optional;nested_optional: Optional[Optional[Optional[str]]] = None
+from typing import Optional;nested_optional: str | None | None = None

Would fix 1 error.
```

This is not clear at all when adding this as a snapshot test. I think we'll need to add a CLI test to make sure that the issue is actually fixed because the snapshots only show one diagnostic and thus one level of fix at a time.

---

_@ntBre reviewed on 2025-08-06 18:04_

I don't think this fully solves the problem. You may also want to look at a related implementation in another rule:

https://github.com/astral-sh/ruff/blob/6b9692cab26ef1858e7db686f6a2e12565bffef9/crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs#L98-L103

---

_Review requested from @ntBre by @danparizher on 2025-08-07 23:18_

---

_Review comment by @ntBre on `crates/ruff/tests/integration_test.rs`:310 on 2025-08-14 14:42_

You can write this as a multi-line string literal:
```suggestion
        "\
from typing import Optional
nested_optional: Optional[Optional[Optional[str]]] = None
"
```

That will make it a little easier to read here and also get rid of the `No newline at end of file` warnings in the output.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep604_annotation.rs`:190 on 2025-08-14 14:46_

Do we need this binding?


```suggestion
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep604_annotation.rs`:176 on 2025-08-14 14:56_

I'm not sure we need this return. Is this just to suppress the fixes for the nested `Optional`s? I think this should be pretty rare, and we're emitting a diagnostic for each one anyway. I think it might be confusing for the inner ones not to have fixes, even if you're unlikely to want to apply those alone.

---

_@ntBre reviewed on 2025-08-14 14:57_

Thanks, I think this is working well now. I just had a few small comments left. Could you fix the merge conflicts too? You probably just need to rerun the tests and accept the snapshots to pull in the new diagnostic format.

---

_Label `bug` added by @ntBre on 2025-08-14 14:58_

---

_Label `fixes` added by @ntBre on 2025-08-14 14:58_

---

_Review requested from @ntBre by @danparizher on 2025-08-15 19:55_

---

_@ntBre approved on 2025-08-19 22:11_

Thank you!

I just pushed one tiny inlining and relocated the CLI test.

---

_Renamed from "[`pyupgrade`] Fix missing `flatten_optional` import in `UP045` rule" to "[`pyupgrade`] Handle nested `Optional`s (`UP045`)" by @ntBre on 2025-08-19 22:12_

---

_Merged by @ntBre on 2025-08-19 22:12_

---

_Closed by @ntBre on 2025-08-19 22:12_

---

_Branch deleted on 2025-08-19 22:48_

---
