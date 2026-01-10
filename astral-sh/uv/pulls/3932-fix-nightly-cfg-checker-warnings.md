```yaml
number: 3932
title: Fix nightly cfg checker warnings
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/fix-nightly-lints
created_at: 2024-05-31T09:26:59Z
updated_at: 2024-05-31T09:35:54Z
url: https://github.com/astral-sh/uv/pull/3932
synced_at: 2026-01-10T13:59:34Z
```

# Fix nightly cfg checker warnings

---

_Pull request opened by @konstin on 2024-05-31 09:26_

Fixes these two warnings on nightly:

```
warning: unexpected `cfg` condition name: `codspeed`
 --> crates/bench/src/lib.rs:5:15
  |
5 |     #[cfg(not(codspeed))]
  |               ^^^^^^^^ help: found config with similar value: `feature = "codspeed"`
  |
  = help: expected names are: `clippy`, `debug_assertions`, `doc`, `docsrs`, `doctest`, `feature`, `miri`, `overflow_checks`, `panic`, `proc_macro`, `relocation_model`, `rustfmt`, `sanitize`, `sanitizer_cfi_generalize_pointers`, `sanitizer_cfi_normalize_integers`, `target_abi`, `target_arch`, `target_endian`, `target_env`, `target_family`, `target_feature`, `target_has_atomic`, `target_has_atomic_equal_alignment`, `target_has_atomic_load_store`, `target_os`, `target_pointer_width`, `target_thread_local`, `target_vendor`, `test`, `ub_checks`, `unix`, and `windows`
  = help: consider using a Cargo feature instead
  = help: or consider adding in `Cargo.toml` the `check-cfg` lint config for the lint:
           [lints.rust]
           unexpected_cfgs = { level = "warn", check-cfg = ['cfg(codspeed)'] }
  = help: or consider adding `println!("cargo::rustc-check-cfg=cfg(codspeed)");` to the top of the `build.rs`
  = note: see <https://doc.rust-lang.org/nightly/rustc/check-cfg/cargo-specifics.html> for more information about checking conditional configuration
  = note: `#[warn(unexpected_cfgs)]` on by default

warning: unexpected `cfg` condition name: `codspeed`
 --> crates/bench/src/lib.rs:8:11
  |
8 |     #[cfg(codspeed)]
  |           ^^^^^^^^ help: found config with similar value: `feature = "codspeed"`
  |
  = help: consider using a Cargo feature instead
  = help: or consider adding in `Cargo.toml` the `check-cfg` lint config for the lint:
           [lints.rust]
           unexpected_cfgs = { level = "warn", check-cfg = ['cfg(codspeed)'] }
  = help: or consider adding `println!("cargo::rustc-check-cfg=cfg(codspeed)");` to the top of the `build.rs`
  = note: see <https://doc.rust-lang.org/nightly/rustc/check-cfg/cargo-specifics.html> for more information about checking conditional configuration
```

```
warning: unexpected `cfg` condition value: `unix`
 --> crates/uv-extract/src/tar.rs:6:16
  |
6 | #[cfg_attr(not(target_os = "unix"), allow(dead_code))]
  |                ^^^^^^^^^^^^^^^^^^
  |
  = note: expected values for `target_os` are: `aix`, `android`, `cuda`, `dragonfly`, `emscripten`, `espidf`, `freebsd`, `fuchsia`, `haiku`, `hermit`, `horizon`, `hurd`, `illumos`, `ios`, `l4re`, `linux`, `macos`, `netbsd`, `none`, `nto`, `openbsd`, `psp`, `redox`, `solaris`, `solid_asp3`, `teeos`, `tvos`, `uefi`, `unknown`, `visionos`, `vita`, `vxworks`, `wasi`, `watchos`, and `windows` and 2 more
  = note: see <https://doc.rust-lang.org/nightly/rustc/check-cfg/cargo-specifics.html> for more information about checking conditional configuration
  = note: requested on the command line with `-W unexpected-cfgs`
```

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

---

_Label `internal` added by @konstin on 2024-05-31 09:27_

---

_Comment by @codspeed-hq[bot] on 2024-05-31 09:32_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/konsti/fix-nightly-lints)

### Merging #3932 will **improve performances by 6.87%**

<sub>Comparing <code>konsti/fix-nightly-lints</code> (9c6364a) with <code>main</code> (081f20c)</sub>



### Summary

`⚡ 1` improvements
`✅ 12` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `konsti/fix-nightly-lints` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `resolve_warm_jupyter` | 92.6 ms | 86.6 ms | +6.87% |


---

_Merged by @konstin on 2024-05-31 09:35_

---

_Closed by @konstin on 2024-05-31 09:35_

---

_Branch deleted on 2024-05-31 09:35_

---
