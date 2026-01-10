```yaml
number: 18056
title: "Update reference documentation for `--python-version`"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: zb/ref-version
created_at: 2025-05-12T20:49:34Z
updated_at: 2025-05-12T22:31:05Z
url: https://github.com/astral-sh/ruff/pull/18056
synced_at: 2026-01-10T18:51:01Z
```

# Update reference documentation for `--python-version`

---

_Pull request opened by @zanieb on 2025-05-12 20:49_

Adding more detail here


---

_Review requested from @carljm by @zanieb on 2025-05-12 20:49_

---

_Review requested from @MichaReiser by @zanieb on 2025-05-12 20:49_

---

_Review requested from @AlexWaygood by @zanieb on 2025-05-12 20:49_

---

_Review requested from @sharkdp by @zanieb on 2025-05-12 20:49_

---

_Label `documentation` added by @zanieb on 2025-05-12 20:49_

---

_Review requested from @dcreager by @zanieb on 2025-05-12 20:49_

---

_Label `ty` added by @zanieb on 2025-05-12 20:49_

---

_Label `documentation` added by @zanieb on 2025-05-12 20:49_

---

_Label `ty` added by @zanieb on 2025-05-12 20:49_

---

_@zanieb reviewed on 2025-05-12 20:50_

---

_Review comment by @zanieb on `crates/ty/src/args.rs`:88 on 2025-05-12 20:50_

I'd like to add this, but it won't make the release tomorrow and this is notable.

---

_Comment by @github-actions[bot] on 2025-05-12 20:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty/src/args.rs`:81 on 2025-05-12 20:58_

it can also affect code in third-party or first-party modules -- it affects any code that uses conditional definitions based on the value of `sys.version_info`. But that's probably too much detail here; we should consider writing a short guide that goes into this in more depth.

---

_@AlexWaygood approved on 2025-05-12 21:00_

---

_@zanieb reviewed on 2025-05-12 21:03_

---

_Review comment by @zanieb on `crates/ty/src/args.rs`:81 on 2025-05-12 21:03_

mm makes sense.

---

_Review comment by @zanieb on `crates/ty/src/args.rs`:81 on 2025-05-12 21:03_

I think we could cover that very briefly here.. hm

---

_@zanieb reviewed on 2025-05-12 21:03_

---

_@AlexWaygood reviewed on 2025-05-12 21:09_

---

_Review comment by @AlexWaygood on `crates/ty/src/args.rs`:81 on 2025-05-12 21:09_

Nice, I love the language you added!

---

_Merged by @zanieb on 2025-05-12 22:31_

---

_Closed by @zanieb on 2025-05-12 22:31_

---

_Branch deleted on 2025-05-12 22:31_

---
