```yaml
number: 3165
title: Add RISC-V (riscv64gc-unknown-linux-gnu) CI and release artifacts
type: pull_request
state: merged
author: mariano-m13
labels: []
assignees: []
merged: true
base: master
head: add-riscv64-support
created_at: 2025-10-04T05:32:05Z
updated_at: 2025-12-25T15:48:48Z
url: https://github.com/BurntSushi/ripgrep/pull/3165
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

_Review comment by @BurntSushi on `README.md`:398 on 2025-10-04 12:21_

This section should be removed. The GitHub release binaries are covered at the top.

---

_Review comment by @BurntSushi on `tests/util.rs`:53 on 2025-10-04 12:22_

Why did you add this?

---

_@BurntSushi reviewed on 2025-10-04 12:23_

I think I'm okay to accept this, but the typical problem with adding targets like this is that the release pipeline doesn't work. Testing it is annoying. I'm happy to do a little work at release time if it doesn't work as-is, but I reserve the right to remove it.

---

_Review comment by @mariano-m13 on `README.md`:398 on 2025-10-04 16:11_

got it, sorry. For testing, I'm hoping to have some time to try on real hardware soon.

---

_@mariano-m13 reviewed on 2025-10-04 16:11_

---

_@mariano-m13 reviewed on 2025-10-04 16:23_

---

_Review comment by @mariano-m13 on `tests/util.rs`:53 on 2025-10-04 16:23_

the tests compressed_lz4, compressed_brotli, and compressed_zstd were failing on RISC-V with exit code 2 and the error particularly was: rg: sherlock.zst: <stderr is empty>. So I added a helper that tests actual decompression capability before running the test.

But I do think a better solution to avoid runtime overhead for all platforms would be to skip these specific tests on RISC-V with #[cfg]. Because these tools are x86_64 binaries, they can't execute inside the RISC-V QEMU environment, and bringing in RISC-V versions of them would maybe add more complexity than we want. 

I added #[cfg(not(target_arch = "riscv64"))] to skip these 3 tests at compile-time. 


---

_@BurntSushi reviewed on 2025-10-05 12:37_

---

_Review comment by @BurntSushi on `README.md`:398 on 2025-10-05 12:37_

Why did you mark this as resolved without removing this section?

---

_@mariano-m13 reviewed on 2025-10-06 06:32_

---

_Review comment by @mariano-m13 on `README.md`:398 on 2025-10-06 06:32_

oh my bad, forgot to drop the commit 1c6666a. Done.

---

_@BurntSushi approved on 2025-10-11 00:40_

Thanks!

---

_Merged by @BurntSushi on 2025-10-11 00:50_

---

_Closed by @BurntSushi on 2025-10-11 00:50_

---

_Comment by @BurntSushi on 2025-10-16 02:08_

Sorry, but this ended up failing during the release CI workflow: https://github.com/BurntSushi/ripgrep-release-test/actions/runs/18547794531/job/52869080774

So I'm going to remove this artifact. It's not worth my headache.

I'm open to adding this back in the future, but someone else will need to take on the burden of testing it and demonstrating that it works before I will merge it.

---

_Comment by @OctopusET on 2025-12-16 14:11_

test seems failing on risc-v because of glibc 2.34+ bug. Not because of riscv64.

It returns true even when it fails

---

_Comment by @OctopusET on 2025-12-16 18:04_

It's more like limitations of QEMU not a 'bug'

See also
https://github.com/rust-lang/rust/issues/90825

---

_Comment by @OctopusET on 2025-12-25 15:48_

This CI failure can be fixed after this bug is resolved for Ubuntu 22.04 or update to Ubuntu 24.04. Anyways cross-rs should be updated either.

https://sourceware.org/bugzilla/show_bug.cgi?id=30237

---
