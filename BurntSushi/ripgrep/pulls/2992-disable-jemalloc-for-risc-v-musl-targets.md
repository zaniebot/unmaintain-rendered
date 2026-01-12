```yaml
number: 2992
title: Disable jemalloc for RISC-V musl targets
type: pull_request
state: closed
author: bwbuhse
labels: []
assignees: []
base: master
head: master
created_at: 2025-02-17T00:22:37Z
updated_at: 2025-08-17T18:28:37Z
url: https://github.com/BurntSushi/ripgrep/pull/2992
synced_at: 2026-01-12T18:23:14Z
```

# Disable jemalloc for RISC-V musl targets

---

_@bwbuhse_

The latest available version of jemallocator doesn't support RISC-V, though support was added and should be available in a future release.

---

_Comment by @BurntSushi on 2025-08-17 18:28_

I'm switching to `tikv-jemallocator`, which [purports to support riscv64](https://github.com/tikv/jemallocator/blob/5c5a9f79b3be3c19e253527f16a091132a455c2e/CHANGELOG.md?plain=1#L44). So I think this will be resolved by #2889. If riscv64 still doesn't work after the next ripgrep release, please feel free to re-open this PR.

---

_Closed by @BurntSushi on 2025-08-17 18:28_

---
