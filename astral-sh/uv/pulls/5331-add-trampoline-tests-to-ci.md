```yaml
number: 5331
title: Add trampoline tests to CI
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/add-uv-trampoline-tests-to-ci
created_at: 2024-07-23T08:18:24Z
updated_at: 2024-07-29T09:43:52Z
url: https://github.com/astral-sh/uv/pull/5331
synced_at: 2026-01-10T13:37:23Z
```

# Add trampoline tests to CI

---

_Pull request opened by @konstin on 2024-07-23 08:18_

Add the tests added in #5204 to CI. The crate is not part of the workspace (it requires nightly) and is windows only, so we have to test it separately.

---

_Label `internal` added by @konstin on 2024-07-23 08:18_

---

_Comment by @samypr100 on 2024-07-23 13:49_

Thanks! Its something I was meaning to do after #5204 got merged 🎉

---

_Comment by @konstin on 2024-07-23 14:41_

The test is currently failing but i don't know why, i have to switch to my windows machine later to figure it out

---

_Review comment by @samypr100 on `.github/workflows/ci.yml`:360 on 2024-07-23 14:47_

```suggestion
        run: cargo test --target ${{ matrix.target-arch }}-pc-windows-msvc --test *
```

Filter for integration tests only?

Alternatevely, we can update Cargo.toml to skip default bins testing

```toml
[[bin]]
name = "uv-trampoline-console"
test = false

[[bin]]
name = "uv-trampoline-gui"
test = false
```

---

_@samypr100 reviewed on 2024-07-23 14:47_

---

_@samypr100 reviewed on 2024-07-27 23:42_

---

_Review comment by @samypr100 on `.github/workflows/ci.yml`:347 on 2024-07-27 23:42_

```suggestion
        target-arch: ["x86_64", "i686"]
```

I don't think aarch64 will run on `windows-latest` runner

---

_Review comment by @samypr100 on `.github/workflows/ci.yml`:365 on 2024-07-27 23:43_

```suggestion
      # Build and copy the new binaries
      - name: "Build"
        working-directory: ${{ github.workspace }}/crates/uv-trampoline
        run: |
          cargo build --target ${{ matrix.target-arch }}-pc-windows-msvc
          cp target/${{ matrix.target-arch }}-pc-windows-msvc/debug/uv-trampoline-console.exe trampolines/uv-trampoline-${{ matrix.target-arch }}-console.exe
          cp target/${{ matrix.target-arch }}-pc-windows-msvc/debug/uv-trampoline-gui.exe trampolines/uv-trampoline-${{ matrix.target-arch }}-gui.exe
      - name: "Test"
```

Worth building and copying the new binaries so that they're tested

---

_@samypr100 reviewed on 2024-07-27 23:43_

---

_Marked ready for review by @konstin on 2024-07-29 09:43_

---

_Merged by @konstin on 2024-07-29 09:43_

---

_Closed by @konstin on 2024-07-29 09:43_

---

_Branch deleted on 2024-07-29 09:43_

---
