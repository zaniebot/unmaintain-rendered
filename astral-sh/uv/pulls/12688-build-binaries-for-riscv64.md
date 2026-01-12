```yaml
number: 12688
title: build-binaries for riscv64 
type: pull_request
state: merged
author: Xeonacid
labels: []
assignees: []
merged: true
base: main
head: riscv64-release
created_at: 2025-04-06T05:19:00Z
updated_at: 2025-06-10T15:18:28Z
url: https://github.com/astral-sh/uv/pull/12688
synced_at: 2026-01-12T16:10:21Z
```

# build-binaries for riscv64 

---

_@Xeonacid_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Build riscv64 binary so it can get released in the GitHub Releases, which is used by many high-level apps.

A copy-paste from linux-s390x, with only target and arch changed.

maturin-action added riscv64 support in v1.48.0, this PR also bumps it to the latest version, v1.48.1.

## Test Plan

<!-- How was it tested? -->

Let CI test itself :P

Already tested in [my fork](https://github.com/Xeonacid/uv/actions/runs/14289179697/job/40048172301)

---

_Review requested from @konstin by @charliermarsh on 2025-04-07 02:05_

---

_Comment by @charliermarsh on 2025-04-07 02:05_

I think we also need to change the cargo-dist manifest in the `Cargo.toml` to include these binaries.

---

_Review requested from @Gankra by @charliermarsh on 2025-04-07 02:05_

---

_Comment by @Xeonacid on 2025-04-07 02:34_

The `cargo test` CI failure seems unrelated to this PR.

---

_Comment by @Xeonacid on 2025-04-07 02:41_

> I think we also need to change the cargo-dist manifest in the `Cargo.toml` to include these binaries.

Thanks for the review! Fixed.

The 2.31 minimal glibc version comes from the [manylinux Docker used by maturin-action](https://github.com/rust-cross/manylinux-cross/pull/101), which is the first manylinux version supported by [auditwheel](https://github.com/pypa/auditwheel/blob/8964d1e5e5daee875c324c2e0b73cc8f3e5af1c9/src/auditwheel/policy/manylinux-policy.json#L326).

---

_Comment by @kxxt on 2025-04-07 04:38_

One thing worth mentioning is that bazel is currently (ab)using the fact that for all currently supported platforms, the target triple in uv releases matches the target triples used in [python-build-standalone](https://github.com/astral-sh/python-build-standalone/releases/tag/20250317).

But as for this PR, the target triple in uv releases (`riscv64gc-unknown-linux-gnu`) would not match `riscv64-unknown-linux-gnu` python-build-standalone.

I think it is up to the uv maintainers to decide whether it is fair for bazel to leverage such a coincidence.
- If it is fair, we need to rename the release artifacts for riscv64 to strip the ISA ext part `gc`.
- If it is not, then bazel's rules_python would need a fix.

---

_Comment by @Gankra on 2025-04-07 18:04_

context on the riscv64/risvc64gc mismatch (note there's also a remapping for "arm5tel"):

* https://github.com/astral-sh/python-build-standalone/issues/504

https://github.com/astral-sh/uv/blob/2b62f73064cd9fdaf4e347d6e835e384d6c6b981/crates/uv-python/src/downloads.rs#L778-L785

---

_Comment by @Gankra on 2025-04-07 18:06_

In this case I think it's fair to ask bazel to fix its mapping.

---

_@Gankra approved on 2025-04-07 18:13_

Implementation-wise this looks correct.

---

_Review comment by @konstin on `.github/workflows/build-binaries.yml`:722 on 2025-04-07 18:16_

The arch is always `riscv64`

---

_@konstin reviewed on 2025-04-07 18:17_

---

_@Xeonacid reviewed on 2025-04-07 23:56_

---

_Review comment by @Xeonacid on `.github/workflows/build-binaries.yml`:722 on 2025-04-07 23:56_

Dropped the two `if` cases. Thanks!

---

_Comment by @Xeonacid on 2025-04-08 00:46_

I have no idea why the s390x build fails, which is untouched between the previous and latest push...

---

_Comment by @konstin on 2025-04-08 08:38_

I restarted the build and it passed, so just a fluke with the runner

---

_Assigned to @Gankra by @konstin on 2025-04-08 10:58_

---

_Comment by @Xeonacid on 2025-04-09 12:27_

Rebased against updated maturin-action.

---

_Comment by @Xeonacid on 2025-04-09 12:55_

The s390x build seems very flaky...

---

_Comment by @Xeonacid on 2025-06-10 12:15_

Hi team, is there any chance to get this PR merged? Thanks!

---

_Comment by @Gankra on 2025-06-10 15:18_

Yep this is ready to land, thanks for the ping!

---

_Merged by @Gankra on 2025-06-10 15:18_

---

_Closed by @Gankra on 2025-06-10 15:18_

---
