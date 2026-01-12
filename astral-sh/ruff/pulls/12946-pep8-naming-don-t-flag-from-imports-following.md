```yaml
number: 12946
title: "[`pep8-naming`] Don't flag `from` imports following conventional import names (`N817`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-n817-from-import
created_at: 2024-08-17T11:34:40Z
updated_at: 2024-08-21T03:59:36Z
url: https://github.com/astral-sh/ruff/pull/12946
synced_at: 2026-01-12T15:55:42Z
```

# [`pep8-naming`] Don't flag `from` imports following conventional import names (`N817`)

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/12940

https://github.com/astral-sh/ruff/pull/12922 changes `N817` to not flag imports that follow an import convention. 
I missed to handle `from .. import` imports as part of this PR. This PR implements the same logic for `from .. import` imports.


## Test Plan

Added test


---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-17 11:34_

---

_Label `bug` added by @MichaReiser on 2024-08-17 11:34_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pep8_naming/rules/camelcase_imported_as_acronym.rs`:100 on 2024-08-17 11:39_

```suggestion
    let full_name = if let Stmt::ImportFrom(import_from) = stmt {
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pep8_naming/rules/camelcase_imported_as_acronym.rs`:108 on 2024-08-17 11:41_

I'd prefer to assert the invariant here: `module` should never be non-`None` if `level == 0` (that would be invalid syntax)

```suggestion
        if import_from.level != 0 {
            return false;
        }
        let module = import_from
            .module
            .as_ref()
            .expect("Expected `module` to always be non-`None` for a non-relative `import from` statement");
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pep8_naming/N817.py`:9 on 2024-08-17 11:46_

I didn't spot the leading dots here at first:

```suggestion
# Always an error (relative import)
```

---

_@AlexWaygood approved on 2024-08-17 11:46_

---

_Comment by @github-actions[bot] on 2024-08-17 11:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-08-17 11:52_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pep8_naming/rules/camelcase_imported_as_acronym.rs`:100 on 2024-08-17 11:52_

I prefer the `as_import_from_stmt`

---

_@MichaReiser reviewed on 2024-08-17 11:52_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pep8_naming/rules/camelcase_imported_as_acronym.rs`:108 on 2024-08-17 11:52_

I prefer the more error resilient code in case we ever change how the parser recovers.

---

_@AlexWaygood reviewed on 2024-08-17 12:00_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pep8_naming/rules/camelcase_imported_as_acronym.rs`:100 on 2024-08-17 12:00_

Interesting -- why? To me it seems like it's more verbose and adds unnecessary indirection here. Obviously not a big issue, just curious!

---

_Merged by @MichaReiser on 2024-08-17 12:05_

---

_Closed by @MichaReiser on 2024-08-17 12:05_

---

_Branch deleted on 2024-08-17 12:05_

---

_Comment by @darosio on 2024-08-21 03:42_

Could you please confirm that this fix is working also for jupyter (.ipynb) files?
Thank you.

---

_Comment by @dhruvmanila on 2024-08-21 03:59_

@darosio I think this should work for Jupyter notebooks as well. Is there an issue that you're facing?

---
