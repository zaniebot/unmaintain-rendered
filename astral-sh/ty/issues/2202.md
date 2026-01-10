```yaml
number: 2202
title: Improve test coverage for iterating over intersections of tuples
type: issue
state: open
author: AlexWaygood
labels:
  - testing
assignees: []
created_at: 2025-12-24T12:34:47Z
updated_at: 2025-12-25T11:27:47Z
url: https://github.com/astral-sh/ty/issues/2202
synced_at: 2026-01-10T01:56:41Z
```

# Improve test coverage for iterating over intersections of tuples

---

_Issue opened by @AlexWaygood on 2025-12-24 12:34_

I don't think we have full test coverage for all code paths in https://github.com/astral-sh/ruff/blob/81f34fbc8e404cd26fa40ee86a339947dce7617c/crates/ty_python_semantic/src/types/tuple.rs#L1794-L1867 currently (added in https://github.com/astral-sh/ruff/commit/0f18a08a0ae848c92bc5524cb17077a9968bbc69)

---

_Label `testing` added by @AlexWaygood on 2025-12-24 12:34_

---

_Comment by @silamon on 2025-12-25 11:27_

Code coverage shows red lines for: 
https://github.com/astral-sh/ruff/blob/81f34fbc8e404cd26fa40ee86a339947dce7617c/crates/ty_python_semantic/src/types/tuple.rs#L1817-L1825
https://github.com/astral-sh/ruff/blob/81f34fbc8e404cd26fa40ee86a339947dce7617c/crates/ty_python_semantic/src/types/tuple.rs#L1846-L1852

---
