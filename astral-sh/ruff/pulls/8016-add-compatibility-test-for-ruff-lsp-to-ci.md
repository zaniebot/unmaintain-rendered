```yaml
number: 8016
title: "Add compatibility test for `ruff-lsp` to CI"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zanie/lsp-check
created_at: 2023-10-17T14:22:07Z
updated_at: 2023-10-23T20:44:52Z
url: https://github.com/astral-sh/ruff/pull/8016
synced_at: 2026-01-12T02:32:41Z
```

# Add compatibility test for `ruff-lsp` to CI

---

_Pull request opened by @zanieb on 2023-10-17 14:22_

Adds a CI job which runs `ruff-lsp` tests against the current Ruff build.

Avoids rebuilding Ruff at the cost of running _after_ the cargo tests have finished. Might be worth the rebuild to get earlier feedback but I don't expect it to fail often?

xref https://github.com/astral-sh/ruff-lsp/pull/286

## Test plan

Verified use of the development version by inspecting version output in CI; supported by https://github.com/astral-sh/ruff-lsp/pull/289 and #8034 

---

_Label `internal` added by @zanieb on 2023-10-17 14:22_

---

_Renamed from "zanie/lsp check" to "Add compatibility test for `ruff-lsp` to CI" by @zanieb on 2023-10-17 14:23_

---

_Marked ready for review by @zanieb on 2023-10-17 15:41_

---

_@charliermarsh reviewed on 2023-10-17 17:28_

This looks reasonable. How did you test that it's using the development version of Rust? Can you add a test plan to the PR summary?

---

_Comment by @zanieb on 2023-10-23 16:37_

A weirdly herculean effort to get it to actually use the development version for testing. Thanks @charliermarsh for the encouragement to verify it thoroughly.


---

_@charliermarsh approved on 2023-10-23 19:59_

---

_Merged by @zanieb on 2023-10-23 20:44_

---

_Closed by @zanieb on 2023-10-23 20:44_

---

_Branch deleted on 2023-10-23 20:44_

---
