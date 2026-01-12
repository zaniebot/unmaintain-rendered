```yaml
number: 1270
title: "Add an `--offline` mode"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/offline
created_at: 2024-02-10T21:05:15Z
updated_at: 2024-02-13T03:35:25Z
url: https://github.com/astral-sh/uv/pull/1270
synced_at: 2026-01-12T16:04:33Z
```

# Add an `--offline` mode

---

_@charliermarsh_

## Summary

This PR adds an `--offline` flag to Puffin that disables network requests (implemented as a Reqwest middleware on our registry client). When `--offline` is provided, we also allow the HTTP cache to return stale data.

Closes #942.


---

_Review comment by @charliermarsh on `Cargo.toml`:89 on 2024-02-10 21:06_

We already have this and `async-trait` in our dependency tree, but they seem to be needed to write Middleware implementations directly.

---

_@charliermarsh reviewed on 2024-02-10 21:06_

---

_Comment by @charliermarsh on 2024-02-10 21:13_

Not ready for review since I want to improve the error messages building on Zanie's work.

---

_Label `enhancement` added by @charliermarsh on 2024-02-10 21:13_

---

_Marked ready for review by @charliermarsh on 2024-02-11 02:01_

---

_Review requested from @zanieb by @charliermarsh on 2024-02-11 02:01_

---

_Review requested from @konstin by @charliermarsh on 2024-02-11 02:01_

---

_Review comment by @charliermarsh on `crates/puffin/tests/pip_compile.rs`:3150 on 2024-02-11 02:03_

@zanieb - This is implemented following your pattern for `NoIndex`, made it very easy.

---

_Review comment by @charliermarsh on `crates/puffin/tests/pip_compile.rs`:3239 on 2024-02-11 02:03_

@zanieb - Do you think this is worth fixing? It likely requires refactoring `--find-links` and threading through the offline result.

---

_@charliermarsh reviewed on 2024-02-11 02:05_

---

_@charliermarsh reviewed on 2024-02-11 02:05_

---

_Review comment by @charliermarsh on `crates/puffin/src/main.rs`:337 on 2024-02-11 02:05_

This isn't _necessarily_ a conflict, e.g., you might want to use `--refresh` with a local package... But I think it's sufficiently confusing to allow both, since what should happen if you try to access a package from the registry?

---

_@charliermarsh reviewed on 2024-02-11 02:21_

---

_Review comment by @charliermarsh on `crates/puffin/tests/pip_compile.rs`:3239 on 2024-02-11 02:21_

I put up a separate PR for it here: https://github.com/astral-sh/puffin/pull/1271

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile.rs`:3150 on 2024-02-12 19:12_

Yay :)

---

_@zanieb reviewed on 2024-02-12 19:12_

---

_@zanieb reviewed on 2024-02-12 19:14_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:509 on 2024-02-12 19:14_

I wonder if we should do a hint per package for this one? It seemed annoying in the `--no-index` case but might be nicer here.

---

_@zanieb approved on 2024-02-12 19:14_

Smooth :)

---

_@charliermarsh reviewed on 2024-02-13 03:24_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/report.rs`:509 on 2024-02-13 03:24_

I could see it either way... I'll leave it as-is for now, it seems annoying if you try to resolve offline with an empty cache and _every_ package is offline.

---

_Merged by @charliermarsh on 2024-02-13 03:35_

---

_Closed by @charliermarsh on 2024-02-13 03:35_

---

_Branch deleted on 2024-02-13 03:35_

---
