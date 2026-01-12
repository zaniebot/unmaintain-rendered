```yaml
number: 3164
title: Add RISC-V (riscv64gc-unknown-linux-gnu) CI and release artifacts
type: pull_request
state: closed
author: mariano-m13
labels: []
assignees: []
base: master
head: add-riscv64-support
created_at: 2025-10-04T05:26:44Z
updated_at: 2025-10-04T05:29:41Z
url: https://github.com/BurntSushi/ripgrep/pull/3164
synced_at: 2026-01-12T18:23:15Z
```

# Add RISC-V (riscv64gc-unknown-linux-gnu) CI and release artifacts

---

_@mariano-m13_

**This PR adds official riscv64 Linux release artifacts so RISC-V developers can run ripgrep natively without cross-compiling.**

## Context & Purpose

**Why**: ripgrep ships binaries for many platforms but not riscv64. Adding a riscv64 GNU artifact gives day-zero parity for RISC-V dev boards and CI and avoids fragmentation. Microsoft's ripgrep-prebuilt already demonstrates feasibility; upstreaming closes the gap and helps hardware teams run standard tools natively without cross-compiling.

**Value for RISC-V hardware teams**: Native rg on boards/SoCs speeds bring-up, enables native/emulated CI jobs on riscv64, and signals downstream packagers to follow.

**Scope philosophy**: Keep it small and safe — CI build + smoke test + GNU release artifacts + a short docs line. No feature changes, no perf guarantees, no musl until explicitly requested.

## Changes

- **CI**: Added `riscv64gc-unknown-linux-gnu` target to the CI workflow matrix, following the same cross-compilation pattern as other architectures (aarch64, armv7, powerpc64, s390x)
- **Release**: Added `riscv64gc-unknown-linux-gnu` target to the release workflow with proper stripping and QEMU emulation for generating completions and man pages
- **Docs**: Added RISC-V Linux installation instructions to README.md with example commands for downloading and installing precompiled binaries

## Testing & Verification

**Local Testing**: Built and tested the riscv64gc-unknown-linux-gnu target locally using `cross`:
- Debug build completed successfully in 18.6s
- Release build completed successfully in 39.6s  
- Binary verified as `ELF 64-bit LSB pie executable, UCB RISC-V, RVC, double-float ABI`
- Successfully ran `rg --version` under QEMU emulation: `ripgrep 14.1.1`
- Confirmed correct dynamic linking with GNU libc (`/lib/ld-linux-riscv64-lp64d.so.1`)

**CI Verification** (on merge):
- [ ] CI job successfully builds `riscv64gc-unknown-linux-gnu` target using cross
- [ ] CI job runs basic smoke tests under QEMU
- [ ] Tagged release includes both `.tar.gz` and `.zip` archives for riscv64
- [ ] Release artifacts include proper `sha256sum.txt` checksums
- [ ] Binary stripping works correctly via Docker cross container
- [ ] Shell completions and man pages are generated via QEMU emulation

## Notes

- **GNU only**: This PR targets `riscv64gc-unknown-linux-gnu` (glibc-based). musl support can be added in a future PR if desired
- **Cross-compilation**: Uses the `cross` tool with QEMU for testing, consistent with other non-x86 targets
- **No source changes**: This is purely a CI/release/docs change with zero modifications to Rust code or features

---

_Comment by @mariano-m13 on 2025-10-04 05:29_

Closing to fix commit author information. Will reopen shortly.

---

_Closed by @mariano-m13 on 2025-10-04 05:29_

---

_Branch deleted on 2025-10-04 05:29_

---
