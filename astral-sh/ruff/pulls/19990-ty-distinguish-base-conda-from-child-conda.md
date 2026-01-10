```yaml
number: 19990
title: "[ty] distinguish base conda from child conda"
type: pull_request
state: merged
author: Gankra
labels:
  - configuration
  - ty
assignees: []
merged: true
base: main
head: gankra/conda-venv
created_at: 2025-08-19T16:11:16Z
updated_at: 2025-08-20T13:07:44Z
url: https://github.com/astral-sh/ruff/pull/19990
synced_at: 2026-01-10T17:52:17Z
```

# [ty] distinguish base conda from child conda

---

_Pull request opened by @Gankra on 2025-08-19 16:11_

This is a port of the logic in https://github.com/astral-sh/uv/pull/7691

The basic idea is we use CONDA_DEFAULT_ENV as a signal for whether CONDA_PREFIX is just the ambient system conda install, or the user has explicitly activated a custom one. If the former, then the conda is treated like a system install (having lowest priority). If the latter, the conda is treated like an activated venv (having priority over everything but an Actual activated venv).

Fixes https://github.com/astral-sh/ty/issues/611

---

_Review requested from @carljm by @Gankra on 2025-08-19 16:11_

---

_Review requested from @AlexWaygood by @Gankra on 2025-08-19 16:11_

---

_Review requested from @sharkdp by @Gankra on 2025-08-19 16:11_

---

_Review requested from @dcreager by @Gankra on 2025-08-19 16:11_

---

_Review requested from @MichaReiser by @Gankra on 2025-08-19 16:11_

---

_Review comment by @Gankra on `crates/ty/tests/cli/python_environment.rs`:883 on 2025-08-19 16:12_

ngl this is so beautifully evil i might cry if y'all reject this

---

_@Gankra reviewed on 2025-08-19 16:12_

---

_Label `ty` added by @Gankra on 2025-08-19 16:12_

---

_Comment by @github-actions[bot] on 2025-08-19 16:13_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Label `configuration` added by @Gankra on 2025-08-19 16:13_

---

_Comment by @github-actions[bot] on 2025-08-19 16:15_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review request for @carljm removed by @carljm on 2025-08-19 17:30_

---

_Comment by @Gankra on 2025-08-19 17:31_

cc @zanieb 

---

_@zanieb reviewed on 2025-08-19 18:28_

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/site_packages.rs`:573 on 2025-08-19 18:28_

N.B. this isn't discussed in your commentary but we only allow these names for base environments. I don't know if people use other default environment names, but there's never been a complaint in uv.

---

_Review comment by @MichaReiser on `crates/ty/tests/cli/python_environment.rs`:883 on 2025-08-20 08:00_

This is fun (and clever). It took me a moment to realise how this all works. An alternative (I don't have a strong preference) which results in fewer diagnostics is to use something like this:

Each virtual environment defines a `package1` that defines a single symbol, environment:

```py
from typing import Final

environment: Final = "active_venv"
```

The main file then becomes

```py
from ty_extensions import static_assert
from package1 import environment

static_assert(environment == "active_venv", "Expected `package1` to resolve to the active environment")
```



---

_@MichaReiser approved on 2025-08-20 08:02_

Thank you. Sorry for commenting on your evil but beautiful test setup :)

---

_@MichaReiser reviewed on 2025-08-20 08:03_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:573 on 2025-08-20 08:03_

This seems straightforward to change if necessary

---

_Comment by @Gankra on 2025-08-20 13:07_

Appreciate the suggestion but the evil lives on :)

(it avoids us having to change the body of main between subtests)

---

_Merged by @Gankra on 2025-08-20 13:07_

---

_Closed by @Gankra on 2025-08-20 13:07_

---

_Branch deleted on 2025-08-20 13:07_

---
