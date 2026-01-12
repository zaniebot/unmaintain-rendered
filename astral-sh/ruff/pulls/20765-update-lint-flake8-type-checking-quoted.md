```yaml
number: 20765
title: "Update `lint.flake8-type-checking.quoted-annotations` docs"
type: pull_request
state: merged
author: CarrotManMatt
labels:
  - documentation
assignees: []
merged: true
base: main
head: update-quoted-annotations-docs
created_at: 2025-10-08T12:41:51Z
updated_at: 2025-10-14T10:43:35Z
url: https://github.com/astral-sh/ruff/pull/20765
synced_at: 2026-01-12T15:57:09Z
```

# Update `lint.flake8-type-checking.quoted-annotations` docs

---

_@CarrotManMatt_

Update `lint.flake8-type-checking.quoted-annotations` docs to mention Python 3.14.


---

_Review comment by @ntBre on `crates/ruff_workspace/src/options.rs`:2171 on 2025-10-10 19:42_

I think we might want to leave the original text and append something like

> Similarly, this setting has no effect on Python versions after 3.14 because  these annotations are also deferred.

It doesn't have a practical difference in most cases, but the 3.14 semantics are a bit different from quoted annotations, so it seems better not to group them together completely. 

My understanding based on PEP 649 is that `from __future__ import annotations` literally turns annotations into strings (requiring them to be `eval`ed or similar if you need to evaluate them), whereas 3.14 leaves them as normal expressions.

---

_@ntBre reviewed on 2025-10-10 19:43_

Thank you! Just one nit about 3.14 vs `__future__` annotations. It also looks like you'll need to run `cargo dev generate-all` to update the schema file, or I can handle that if you prefer :)

---

_Label `documentation` added by @ntBre on 2025-10-10 19:43_

---

_Review comment by @CarrotManMatt on `crates/ruff_workspace/src/options.rs`:2171 on 2025-10-10 22:52_

Thanks for the review!💚

Yep, agreed that keeping the wording separate reflects the difference in outcomes from using PEP 649 annotations vs `from __future__ import annotations`.
PEP 649 annotations aren't exactly left as normal expressions, they're held unevaluated internally until a view of them is explicitly requested with one of the `annotationlib.Format`s (one of which is complete evaluation to normal expressions). But that's unnecessary nit-picking that doesn't need to be mentioned in the docs.

In your opinion is it worth calling the 3.14+ annotations `PEP 649 annotations` or `PEP 649/PEP 749 annotations` as PEP 749 was just the confirmation for how to internally implement the annotation structure defined in PEP 649.

---

_@CarrotManMatt reviewed on 2025-10-10 22:52_

---

_Comment by @CarrotManMatt on 2025-10-10 22:53_

I'd appreciate if you could perform the docs generation for me after I make the requested change.

---

_Comment by @MichaReiser on 2025-10-14 06:40_

Thank you

---

_Merged by @MichaReiser on 2025-10-14 06:43_

---

_Closed by @MichaReiser on 2025-10-14 06:43_

---

_Comment by @github-actions[bot] on 2025-10-14 06:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
