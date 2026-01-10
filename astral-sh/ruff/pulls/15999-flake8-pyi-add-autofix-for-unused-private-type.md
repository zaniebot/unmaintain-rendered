```yaml
number: 15999
title: "[flake8-pyi] Add autofix for unused-private-type-var (PYI018)"
type: pull_request
state: merged
author: ayushbaweja
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: fix-pyi018-unused-private-type-var
created_at: 2025-02-06T17:16:38Z
updated_at: 2025-02-06T18:10:36Z
url: https://github.com/astral-sh/ruff/pull/15999
synced_at: 2026-01-10T19:57:22Z
```

# [flake8-pyi] Add autofix for unused-private-type-var (PYI018)

---

_Pull request opened by @ayushbaweja on 2025-02-06 17:16_

Resolves [#15940](https://github.com/astral-sh/ruff/issues/15940)

## Summary

This pull request adds an autofix for the `flake8-pyi` rule PYI018 (Unused private type variable).  Currently, Ruff detects and reports unused private type variables, but it doesn't offer a way to automatically remove them. This change addresses this by providing an autofix that deletes the unused type variable definition.

---

_Review requested from @AlexWaygood by @ayushbaweja on 2025-02-06 17:16_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/unused_private_type_definition.rs`:237 on 2025-02-06 17:28_

a couple of small things:
1. Let's first make this fix [preview](https://docs.astral.sh/ruff/preview/)-only. If it's been a few months and we haven't had any bug reports about it, we can promote it so that it's also available in stable mode
2. I think this fix has to be marked as unsafe, unfortunately. There's always a chance that somebody is importing the TypeVar from another module, even if it's marked as private, and this fix would break your code if you're doing that.

```suggestion
        if checker.settings.preview.is_enabled() {
	        let edit = fix::edits::delete_stmt(stmt, None, checker.locator(), checker.indexer());
	        diagnostic.set_fix(Fix::unsafe_edit(edit));
	    }

        diagnostics.push(diagnostic);
```

This might mean that you have to change the `FIX_AVAILABILITY` constant on line 34 to `FIX_AVAILABILITY::SOMETIMES`

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI018.py`:1 on 2025-02-06 17:28_

hmm, were the changes to this file deliberate?

---

_@AlexWaygood reviewed on 2025-02-06 17:29_

Thanks, this looks great!

---

_Label `fixes` added by @AlexWaygood on 2025-02-06 17:29_

---

_@ayushbaweja reviewed on 2025-02-06 17:43_

---

_Review comment by @ayushbaweja on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI018.py`:1 on 2025-02-06 17:43_

Autodeleted unused type variables, fixing this now

---

_Comment by @github-actions[bot] on 2025-02-06 17:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2025-02-06 17:54_

Thank you! I'll push a few small fixups and then land 👍

---

_Label `preview` added by @AlexWaygood on 2025-02-06 18:03_

---

_Merged by @AlexWaygood on 2025-02-06 18:08_

---

_Closed by @AlexWaygood on 2025-02-06 18:08_

---

_Branch deleted on 2025-02-06 18:10_

---
