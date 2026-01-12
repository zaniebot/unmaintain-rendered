```yaml
number: 1856
title: Refactor import-tracking to leverage existing AST bindings
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/import-model
created_at: 2023-01-14T01:13:59Z
updated_at: 2023-01-14T01:39:55Z
url: https://github.com/astral-sh/ruff/pull/1856
synced_at: 2026-01-12T15:55:07Z
```

# Refactor import-tracking to leverage existing AST bindings

---

_@charliermarsh_

This PR refactors our import-tracking logic to leverage our existing logic for tracking bindings. It's both a significant simplification, a significant improvement (as we can now track reassignments), and closes out a bunch of subtle bugs.

Though the AST tracks all bindings (e.g., when parsing `import os as foo`, we bind the name `foo` to a `BindingKind::Importation` that points to the `os` module), when I went to implement import tracking (e.g., to ensure that if the user references `List`, it's actually `typing.List`), I added a parallel system specifically for this use-case.

That was a mistake, for a few reasons:

1. It didn't track reassignments, so if you had `from typing import List`, but `List` was later overridden, we'd still consider any reference to `List` to be `typing.List`.
2. It required a bunch of extra logic, include complex logic to try and optimize the lookups, since it's such a hot codepath.
3. There were a few bugs in the implementation that were just hard to correct under the existing abstractions (e.g., if you did `from typing import Optional as Foo`, then we'd treat any reference to `Foo` _or_ `Optional` as `typing.Optional` (even though, in that case, `Optional` was really unbound).

The new implementation goes through our existing binding tracking: when we get a reference, we find the appropriate binding given the current scope stack, and normalize it back to its original target.

Closes #1690.
Closes #1790.


---

_Renamed from "Refactor import-tracking to leverage AST bindings" to "Refactor import-tracking to leverage existing AST bindings" by @charliermarsh on 2023-01-14 01:19_

---

_Merged by @charliermarsh on 2023-01-14 01:39_

---

_Closed by @charliermarsh on 2023-01-14 01:39_

---

_Branch deleted on 2023-01-14 01:39_

---
