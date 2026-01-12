```yaml
number: 8429
title: "Improve detection of `TYPE_CHECKING` blocks imported from `typing_extensions` or `_typeshed`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zanie/type-checking
created_at: 2023-11-02T01:32:39Z
updated_at: 2023-11-09T18:21:04Z
url: https://github.com/astral-sh/ruff/pull/8429
synced_at: 2026-01-12T15:55:26Z
```

# Improve detection of `TYPE_CHECKING` blocks imported from `typing_extensions` or `_typeshed`

---

_@zanieb_

~Improves detection of types imported from `typing_extensions`. Removes the hard-coded list of supported types in `typing_extensions`; instead assuming all types could be imported from `typing`, `_typeshed`, or `typing_extensions`.~

~The typing extensions package appears to re-export types even if they do not need modification.~


Adds detection of `if typing_extensions.TYPE_CHECKING` blocks. Avoids inserting a new `if TYPE_CHECKING` block and `from typing import TYPE_CHECKING` if `typing_extensions.TYPE_CHECKING` is used (closes https://github.com/astral-sh/ruff/issues/8427)

---

_Comment by @zanieb on 2023-11-02 01:33_

I'd like to update some test fixtures

---

_Comment by @zanieb on 2023-11-02 01:35_

It looks like at https://github.com/astral-sh/ruff/blob/1e173f7909f9c78b38741cd510b65153dd408247/crates/ruff_linter/src/importer/mod.rs#L135-L139 we need to check if a `typing_extensions.TYPE_CHECKING` block is used before inserting the `typing.TYPE_CHECKING` block to actually solve the issue.

---

_@charliermarsh reviewed on 2023-11-02 02:01_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/analyze/typing.rs`:319 on 2023-11-02 02:01_

We may want to instead use `semantic.match_typing_call_path` which checks `typing_extensions`, but also `_typeshed` and any user-configured modules... Though I don't know if type checkers respect `TYPING_CHECKING` re-exports? I assume so.

---

_@zanieb reviewed on 2023-11-02 02:03_

---

_Review comment by @zanieb on `crates/ruff_python_semantic/src/analyze/typing.rs`:319 on 2023-11-02 02:03_

That's a good idea I'll look into that too

---

_Comment by @github-actions[bot] on 2023-11-09 04:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2023-11-09 04:40_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:186 on 2023-11-09 04:40_

I decided to remove this behavior in which we check if the member is part of `typing_extensions`. We could retain it... but that list may vary over time, so hard-coding it makes Ruff less future-proof.

---

_Marked ready for review by @charliermarsh on 2023-11-09 04:40_

---

_Comment by @charliermarsh on 2023-11-09 04:40_

@zanieb - I decided to finish this one up -- feel free to review and merge.

---

_@zanieb reviewed on 2023-11-09 06:19_

---

_Review comment by @zanieb on `crates/ruff_python_semantic/src/model.rs`:186 on 2023-11-09 06:19_

Hm what's the effect of this then? Is it a breaking change?

---

_Review comment by @zanieb on `crates/ruff_python_semantic/src/model.rs`:180 on 2023-11-09 06:34_

fyi using `matches!` really broke things because `target` here isn't what you wanted — it's capturing second item in the slice not using the provided `target`.

---

_@zanieb reviewed on 2023-11-09 06:34_

---

_@zanieb reviewed on 2023-11-09 06:35_

---

_Review comment by @zanieb on `crates/ruff_python_semantic/src/model.rs`:180 on 2023-11-09 06:35_

fixed in https://github.com/astral-sh/ruff/pull/8429/commits/545bd30c7bdc50009cf931ad585884cdd2117535 — not sure how else to write it

---

_Renamed from "Add `typing_extensions.TYPE_CHECKING` to detection of type checking blocks" to "Improves detection of types imported from `typing_extensions`" by @zanieb on 2023-11-09 06:38_

---

_Label `bug` added by @zanieb on 2023-11-09 06:43_

---

_@charliermarsh reviewed on 2023-11-09 14:29_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:186 on 2023-11-09 14:29_

It's not breaking, it's _more_ lax (the net effect is that we consider all imports from `typing_extensions` to be treated equivalently to `typing`, rather than the allow-list that we encoded of specific members). But let's back it out.

---

_Renamed from "Improves detection of types imported from `typing_extensions`" to "Improve detection of `TYPE_CHECKING` blocks imported from `typing_extensions` or `_typeshed`" by @zanieb on 2023-11-09 14:33_

---

_Merged by @zanieb on 2023-11-09 18:21_

---

_Closed by @zanieb on 2023-11-09 18:21_

---

_Branch deleted on 2023-11-09 18:21_

---
