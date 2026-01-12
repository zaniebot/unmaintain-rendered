```yaml
number: 3643
title: "CI: Add CI step to check for unused dependencies"
type: issue
state: closed
author: MichaReiser
labels:
  - internal
assignees: []
created_at: 2023-03-21T08:49:59Z
updated_at: 2023-03-22T14:53:14Z
url: https://github.com/astral-sh/ruff/issues/3643
synced_at: 2026-01-12T15:54:43Z
```

# CI: Add CI step to check for unused dependencies

---

_@MichaReiser_

Add a new CI job to the [`ci.yaml`](https://github.com/charliermarsh/ruff/blob/main/.github/workflows/ci.yaml) workflow that checks for unused dependencies using [`cargo udeps`](https://github.com/est31/cargo-udeps). It will help us keeping the `[dependencies]` and `[dev-dependencies]` sections accurate. 

> Note: You can use [install-development-tools](https://github.com/marketplace/actions/install-development-tools) to install `cargo-udeps` instead of building it from source. 

---

_Label `internal` added by @charliermarsh on 2023-03-21 23:15_

---

_Closed by @MichaReiser on 2023-03-22 14:53_

---
