```yaml
number: 5715
title: Use fast build profile in more CI jobs
type: pull_request
state: closed
author: zanieb
labels:
  - testing
assignees: []
draft: true
base: main
head: zb/test-fast-build
created_at: 2024-08-01T23:11:10Z
updated_at: 2024-08-07T20:56:46Z
url: https://github.com/astral-sh/uv/pull/5715
synced_at: 2026-01-10T13:31:54Z
```

# Use fast build profile in more CI jobs

---

_Pull request opened by @zanieb on 2024-08-01 23:11_

Part of https://github.com/astral-sh/uv/issues/5713

---

_Label `testing` added by @zanieb on 2024-08-01 23:11_

---

_Renamed from "Use fast build mode for test runs" to "Use fast build profile in more CI jobs" by @zanieb on 2024-08-01 23:25_

---

_@samypr100 reviewed on 2024-08-02 01:34_

---

_Review comment by @samypr100 on `.github/workflows/ci.yml`:381 on 2024-08-02 01:34_

```suggestion
          cargo build --target ${{ matrix.target-arch }}-pc-windows-msvc
          cp target/${{ matrix.target-arch }}-pc-windows-msvc/debug/uv-trampoline-console.exe trampolines/uv-trampoline-${{ matrix.target-arch }}-console.exe
          cp target/${{ matrix.target-arch }}-pc-windows-msvc/debug/uv-trampoline-gui.exe trampolines/uv-trampoline-${{ matrix.target-arch }}-gui.exe
```

fast-build profile is not supported in uv-trampoline yet 😅 

---

_Closed by @zanieb on 2024-08-07 20:56_

---
