```yaml
number: 2097
title: Bazel rules
type: issue
state: open
author: SamuelMarks
labels:
  - wish
assignees: []
created_at: 2025-12-19T01:28:16Z
updated_at: 2025-12-19T09:39:21Z
url: https://github.com/astral-sh/ty/issues/2097
synced_at: 2026-01-12T15:54:26Z
```

# Bazel rules

---

_@SamuelMarks_

[Bazel](https://bazel.build) is a popular cross-platform cross-language build system. E.g., at Google it is commonly and publicly known that essentially every [sub]repo in its monorepo has `BUILD` files for Bazel integration.

[ty](https://github.com/astral-sh/ty) could easily become the replacement for [pytype](https://github.com/google/pytype) now that that's deprecated.

To that end, the recommendation is for the creation and maintenance of `rules_ty`.

FWIW: There is https://github.com/aspect-build/rules_py but it seems to have some weirdness associated requiring a login to their SaaS.

References:
- https://github.com/bazel-contrib/rules_python
- https://github.com/bazel-contrib/rules_mypy
- https://github.com/bazel-contrib/rules_python/issues/296 (call for action now that pytype—Google's oft used solution—is deprecated)

---

_Comment by @MichaReiser on 2025-12-19 09:05_

Thanks for this suggestion.

We don't currently have the capacity or expertise on the team to maintain those rules ourselves. But more than happy to support any work within ty that unblocks a community-driven ty buck rule set.  

---

_Label `wish` added by @MichaReiser on 2025-12-19 09:06_

---

_Renamed from "bazel astral-sh/ty à la https://github.com/bazel-contrib/rules_mypy" to "Bazel rules" by @MichaReiser on 2025-12-19 09:06_

---

_Comment by @zsol on 2025-12-19 09:39_

Would you mind elaborating on the Aspect Build SaaS login weirdness you're seeing? 

---
