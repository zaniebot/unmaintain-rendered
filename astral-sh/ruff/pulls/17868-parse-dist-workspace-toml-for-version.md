```yaml
number: 17868
title: "Parse `dist-workspace.toml` for version"
type: pull_request
state: merged
author: zanieb
labels:
  - ty
assignees: []
merged: true
base: main
head: zb/version-ii
created_at: 2025-05-05T19:06:19Z
updated_at: 2025-05-06T12:18:19Z
url: https://github.com/astral-sh/ruff/pull/17868
synced_at: 2026-01-12T15:56:07Z
```

# Parse `dist-workspace.toml` for version

---

_@zanieb_

Extends https://github.com/astral-sh/ruff/pull/17866, using `dist-workspace.toml` as a source of truth for versions to enable version retrieval in distributions that are not Git repositories (i.e., Python source distributions and source tarballs consumed by Linux distros).

I retain the Git tag lookup from https://github.com/astral-sh/ruff/pull/17866 as a fallback — it seems harmless, but we could drop it to simplify things here.

I confirmed this works from the repository as well as Python source and binary distributions:

```
❯ uv run --refresh-package ty --reinstall-package ty -q --  ty version
ty 0.0.1-alpha.1+5 (2eadc9e61 2025-05-05)
❯ uv build
...
❯ uvx --from ty@dist/ty-0.0.0a1.tar.gz --no-cache -q -- ty version
ty 0.0.1-alpha.1
❯ uvx --from ty@dist/ty-0.0.0a1-py3-none-macosx_11_0_arm64.whl -q -- ty version
ty 0.0.1-alpha.1
```

Requires https://github.com/astral-sh/ty/pull/36

cc @Gankra and @MichaReiser for review.

---

_Review requested from @Gankra by @zanieb on 2025-05-05 19:10_

---

_Review comment by @Gankra on `crates/ty/build.rs`:24 on 2025-05-05 19:13_

I think you have to check the parent dir actually (and consider it not an error for no such file to exist).

---

_@Gankra reviewed on 2025-05-05 19:13_

---

_@zanieb reviewed on 2025-05-05 19:14_

---

_Review comment by @zanieb on `crates/ty/build.rs`:24 on 2025-05-05 19:14_

wdym?

The parent dir change is in #17866 

---

_Comment by @github-actions[bot] on 2025-05-05 19:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @zanieb on 2025-05-05 20:42_

---

_Review requested from @carljm by @zanieb on 2025-05-05 20:42_

---

_Review requested from @MichaReiser by @zanieb on 2025-05-05 20:42_

---

_Review requested from @AlexWaygood by @zanieb on 2025-05-05 20:42_

---

_Review requested from @sharkdp by @zanieb on 2025-05-05 20:42_

---

_Review requested from @dcreager by @zanieb on 2025-05-05 20:42_

---

_@Gankra reviewed on 2025-05-05 21:46_

---

_Review comment by @Gankra on `crates/ty/build.rs`:24 on 2025-05-05 21:46_

Ah cool, perfect.

---

_@Gankra approved on 2025-05-05 21:46_

---

_Label `ty` added by @MichaReiser on 2025-05-06 06:45_

---

_@MichaReiser approved on 2025-05-06 06:47_

---

_Merged by @zanieb on 2025-05-06 12:18_

---

_Closed by @zanieb on 2025-05-06 12:18_

---

_Branch deleted on 2025-05-06 12:18_

---
