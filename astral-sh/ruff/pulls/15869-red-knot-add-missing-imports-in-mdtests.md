```yaml
number: 15869
title: "[red-knot] Add missing imports in mdtests"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/add-missing-imports
created_at: 2025-02-01T10:24:07Z
updated_at: 2025-02-03T09:27:36Z
url: https://github.com/astral-sh/ruff/pull/15869
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Add missing imports in mdtests

---

_Pull request opened by @dhruvmanila on 2025-02-01 10:24_

## Summary

Related to #15848, this PR adds the imports explicitly as we'll now flag these symbols as undefined.


---

_Label `red-knot` added by @dhruvmanila on 2025-02-01 10:24_

---

_Review requested from @carljm by @dhruvmanila on 2025-02-01 10:24_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-01 10:24_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-02-01 10:24_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-02-01 10:24_

---

_@dhruvmanila reviewed on 2025-02-01 10:26_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:39 on 2025-02-01 10:26_

I'm curious to know why is this using `typing_extensions`? I've seen mixed usage of `typing_extensions` and `typing`

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/directives/assert_type.md`:82 on 2025-02-01 11:16_

tiny nit: you can import them both from `typing_extensions`, since everything in `typing` is reexported by `typing_extensions`. It makes the setup for the test slightly less verbose:

```suggestion
from typing_extensions import Any, assert_type
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:39 on 2025-02-01 11:19_

It's because `Never` was only added to the `typing` module on Python 3.11, but it's available from `typing_extensions` on all Python versions. We assume Python 3.9 as our default target version, so importing `Never` from `typing` would only work if we had an `environment` toml block setting the `python-version` setting to `"3.11"` or higher.

Red-knot thinks that `typing_extensions` is part of the standard library, because it's included in typeshed's `stdlib/` directory, and `typing_extensions` re-exports everything that's included in the `typing` module, so it's often easier in mdtests to import things from `typing_extensions` rather than `typing`.

Here I'd probably import them both from `typing_extensions`, since it makes the mdtest shorter by a line:

```suggestion
from typing_extensions import Literal, Never
```

---

_@AlexWaygood approved on 2025-02-01 11:20_

Thank you!

---

_@sharkdp approved on 2025-02-03 07:46_

---

_@MichaReiser approved on 2025-02-03 08:44_

Thanks for taking the time to split this change into its own PR! 

---

_@dhruvmanila reviewed on 2025-02-03 09:21_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:39 on 2025-02-03 09:21_

Thank you for the explanation, that makes sense.

---

_Merged by @dhruvmanila on 2025-02-03 09:27_

---

_Closed by @dhruvmanila on 2025-02-03 09:27_

---

_Branch deleted on 2025-02-03 09:27_

---
