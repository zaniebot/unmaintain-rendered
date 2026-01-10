```yaml
number: 13429
title: "Use `deflate` on PowerPC"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/zstd
created_at: 2024-09-20T20:12:43Z
updated_at: 2024-09-21T08:07:25Z
url: https://github.com/astral-sh/ruff/pull/13429
synced_at: 2026-01-10T21:30:32Z
```

# Use `deflate` on PowerPC

---

_Pull request opened by @charliermarsh on 2024-09-20 20:12_

## Summary

I think this should work... But it's hard to test (see: https://github.com/conda-forge/ruff-feedstock/pull/219). AFAICT, those builds still happen on an X86 machine, so it should be okay that the build script _depends_ on zstd, as long as it doesn't have to pull it in.

---

_Review requested from @carljm by @charliermarsh on 2024-09-20 20:12_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-09-20 20:12_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-09-20 20:12_

---

_Comment by @github-actions[bot] on 2024-09-20 20:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @zanieb on 2024-09-20 20:58_

Tested at https://github.com/conda-forge/ruff-feedstock/pull/220

---

_Comment by @charliermarsh on 2024-09-20 21:24_

Alas

---

_Comment by @charliermarsh on 2024-09-20 21:25_

Three options:

1. Don't use zstd even when building for PowerPC (remove it from the build script).
2. Stop supporting PowerPC for now.
3. Find a way to fix the build issue on conda-forge (best but least unsure how to do).

---

_Comment by @charliermarsh on 2024-09-20 21:29_

Current version of the PR attempts (1), lets see if it makes a difference.

---

_Comment by @charliermarsh on 2024-09-20 21:41_

I don't really get it, but that also didn't work. I think because the host machine target triple isn't PowerPC?

---

_Comment by @charliermarsh on 2024-09-20 21:48_

I think we need to solve this within the conda-forge build.

---

_Closed by @charliermarsh on 2024-09-20 21:48_

---

_Comment by @MichaReiser on 2024-09-21 07:29_

You also need to change the build.rs here

https://github.com/astral-sh/ruff/blob/6c303b24455f26ff08859affc080e43e1206ea6c/crates/red_knot_python_semantic/build.rs#L33

---

_Comment by @MichaReiser on 2024-09-21 08:07_

I guess the problem here is that conda cross compiles to power pc. That's why the conditional target_arch build dependencies don't work

---
