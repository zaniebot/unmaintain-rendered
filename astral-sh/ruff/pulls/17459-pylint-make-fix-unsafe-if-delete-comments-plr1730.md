```yaml
number: 17459
title: "[`pylint`] make fix unsafe if delete comments (`PLR1730`)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - fixes
assignees: []
merged: true
base: main
head: issue-17311
created_at: 2025-04-18T12:51:19Z
updated_at: 2025-04-18T16:49:02Z
url: https://github.com/astral-sh/ruff/pull/17459
synced_at: 2026-01-12T15:56:02Z
```

# [`pylint`] make fix unsafe if delete comments (`PLR1730`)

---

_@VascoSch92_

The PR dresses issue #17311 


---

_Comment by @github-actions[bot] on 2025-04-18 12:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/if_stmt_min_max.rs`:44 on 2025-04-18 14:48_

I don't think we generally include examples like this, but I don't really mind it if you want to keep it. The `preserved comment` here is technically after the replacement range, so it's a bit redundant with the explanation in your first line, but that's probably not obvious to most users.
```suggestion
/// An example to illustrate where comments are preserved and where they are not:
///
/// ```py
/// a, b = 0, 10
///
/// if a >= b: # deleted comment
///     # deleted comment
///     a = b # preserved comment
/// ```
///
```

If we do keep the example, I think this should just be `py`. I believe `pyi` is for stub files, where code like this would be pretty unusual, as far as I know.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/if_stmt_min_max.rs`:185 on 2025-04-18 14:48_

nit: we should factor out `stmt_if.range()` into a variable since we use it here and in the `Edit::range_replacement` call.

---

_@ntBre approved on 2025-04-18 14:51_

Thanks! This looks good, just two nits (and you may need to reformat the example code block to pass CI, if we decide to keep it)

---

_Comment by @VascoSch92 on 2025-04-18 15:32_

Hey @ntBre 
I addressed your feedbacks. I hope it is what you wanted. 

For the example: I took inspiration from [here](https://github.com/VascoSch92/ruff/blob/44ad2012622d73373eb7922978f391eb6c1e54fa/crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs#L36) 
But I can delete it. No problem ;-) 

PS: the CI is failing because of some red knot stuff. I don't know how this is related to my changes

---

_Comment by @ntBre on 2025-04-18 15:59_

> Hey @ntBre I addressed your feedbacks. I hope it is what you wanted.

Thanks!

> For the example: I took inspiration from [here](https://github.com/VascoSch92/ruff/blob/44ad2012622d73373eb7922978f391eb6c1e54fa/crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs#L36) But I can delete it. No problem ;-)

That rule applies to stub files, and  the `## Example` section also uses `pyi`. This rule is for non-stub files, so we should use `py` or `python` in the code blocks.

> PS: the CI is failing because of some red knot stuff. I don't know how this is related to my changes

Yeah no worries, `main` was actually failing for a little while, and your PR must have picked that up. I'll close and re-open the PR to retrigger CI because I think this is good to land otherwise.


---

_Closed by @ntBre on 2025-04-18 15:59_

---

_Reopened by @ntBre on 2025-04-18 15:59_

---

_@ntBre approved on 2025-04-18 16:48_

---

_Label `fixes` added by @ntBre on 2025-04-18 16:48_

---

_Merged by @ntBre on 2025-04-18 16:49_

---

_Closed by @ntBre on 2025-04-18 16:49_

---
