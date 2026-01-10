```yaml
number: 1857
title: Verify commited trampolines are up to date
type: pull_request
state: closed
author: MichaReiser
labels:
  - internal
assignees: []
draft: true
base: main
head: verify-commited-trampoline
created_at: 2024-02-22T06:47:28Z
updated_at: 2024-02-22T13:40:27Z
url: https://github.com/astral-sh/uv/pull/1857
synced_at: 2026-01-10T14:54:43Z
```

# Verify commited trampolines are up to date

---

_Pull request opened by @MichaReiser on 2024-02-22 06:47_

## Summary

Verify that the commited trampolines are up to date because it's easy to either copy the wrong binaries or forget to update one of them.

## Test Plan

* A failed build because the trampolines were out of date [run](https://github.com/astral-sh/uv/actions/runs/8000567845/job/21850205774?pr=1857).
* 


---

_Label `internal` added by @MichaReiser on 2024-02-22 06:50_

---

_Comment by @MichaReiser on 2024-02-22 13:40_

Rust doesn't generate stable binaries (may include references to paths etc so this is somewhat more involved)

---

_Closed by @MichaReiser on 2024-02-22 13:40_

---
