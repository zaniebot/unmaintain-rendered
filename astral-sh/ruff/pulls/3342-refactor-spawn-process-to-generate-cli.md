```yaml
number: 3342
title: "refactor: Spawn process to generate CLI documentation"
type: pull_request
state: closed
author: MichaReiser
labels:
  - internal
assignees: []
base: main
head: spawn-process-for-help-documentation
created_at: 2023-03-04T17:53:00Z
updated_at: 2023-03-11T16:58:57Z
url: https://github.com/astral-sh/ruff/pull/3342
synced_at: 2026-01-12T04:39:44Z
```

# refactor: Spawn process to generate CLI documentation

---

_Pull request opened by @MichaReiser on 2023-03-04 17:53_

This PR spawns the `ruff_cli` in the `dev generate-cli-help` command instead of linking against it. The benefit is that it removes the need that `ruff_cli` is a binary and library crate.

My main motivation was that clippy ended up generating a ton of unused warnings when I moved code from `ruff` to `ruff_cli` that only was used in the binary crate. This could be worked around by splitting the code into more modules but that feels like unnecessary extra work.

The main downside of this approach is that it potentially is slower because `cargo` sometimes happens to build the CLI again.

---

_Label `internal` added by @MichaReiser on 2023-03-04 18:01_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-03-04 18:01_

---

_@charliermarsh approved on 2023-03-05 04:05_

---

_Comment by @MichaReiser on 2023-03-08 07:43_

Closing, approach is no longer feasible after #3320 

---

_Closed by @MichaReiser on 2023-03-08 07:43_

---

_Branch deleted on 2023-03-11 16:58_

---
