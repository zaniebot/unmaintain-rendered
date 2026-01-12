```yaml
number: 1583
title: Building with both simd and pcre2 currently fails
type: issue
state: closed
author: assarbad
labels: []
assignees: []
created_at: 2020-05-15T08:23:15Z
updated_at: 2020-05-27T07:28:19Z
url: https://github.com/BurntSushi/ripgrep/issues/1583
synced_at: 2026-01-12T16:13:23Z
```

# Building with both simd and pcre2 currently fails

---

_@assarbad_

#### What version of ripgrep are you using?

v12.1.0

#### How did you install ripgrep?

I did not install it yet, as the build fails.

#### What operating system are you using ripgrep on?

```
$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 18.04.4 LTS
Release:        18.04
Codename:       bionic
```

#### Describe your bug.

The build fails when attempting the build as follows:

```
RUSTFLAGS="-C target-cpu=native" cargo build --release --features 'pcre2 simd-accel'
```

My CPU supports SIMD, but this should not even matter _during compilation_:

```
$ grep -i avx /proc/cpuinfo|sort|uniq
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc art arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc cpuid aperfmperf pni pclmulqdq dtes64 monitor ds_cpl vmx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid dca sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm 3dnowprefetch cpuid_fault epb cat_l3 cdp_l3 invpcid_single pti ssbd mba ibrs ibpb stibp tpr_shadow vnmi flexpriority ept vpid ept_ad fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm cqm mpx rdt_a avx512f avx512dq rdseed adx smap clflushopt clwb intel_pt avx512cd avx512bw avx512vl xsaveopt xsavec xgetbv1 xsaves cqm_llc cqm_occup_llc cqm_mbm_total cqm_mbm_local dtherm ida arat pln pts hwp hwp_act_window hwp_epp hwp_pkg_req md_clear flush_l1d
```

#### What are the steps to reproduce the behavior?

See below.

#### What is the actual behavior?

```
RUSTFLAGS="-C target-cpu=native" cargo build --release --features 'pcre2 simd-accel'
```

... the failure is (relevant excerpt):

```
  ...
   Compiling bstr v0.2.12
error[E0554]: `#![feature]` may not be used on the stable release channel
   --> /home/oliver/.cargo/registry/src/github.com-1ecc6299db9ec823/packed_simd-0.3.3/src/lib.rs:202:1
    |
202 | / #![feature(
203 | |     repr_simd,
204 | |     const_fn,
205 | |     platform_intrinsics,
...   |
215 | |     custom_inner_attributes
216 | | )]
    | |__^

   Compiling regex v1.3.7
   Compiling globset v0.4.5
   Compiling grep-regex v0.1.8
   Compiling pcre2-sys v0.2.2
   Compiling grep-cli v0.1.4
   Compiling ignore v0.4.15
   Compiling serde_json v1.0.52
   Compiling serde_derive v1.0.107
   Compiling ripgrep v12.1.0 (/home/oliver/.cargo/registry/src/github.com-1ecc6299db9ec823/ripgrep-12.1.0)
error: aborting due to previous error

For more information about this error, try `rustc --explain E0554`.
error: could not compile `packed_simd`.

To learn more, run the command again with --verbose.
warning: build failed, waiting for other jobs to finish...
error: build failed
```

#### What is the expected behavior?

The expected behavior would be for the build to succeed.

----

I'll just revert to the default without SIMD, which builds. I simply wanted to bring this to your attention, since I didn't find any ticket (closed or not) which mentioned `packed_simd`. Apologies in advance, for the case the GitHub search failed me and this ticket ends up creating _just_ noise.

With best regards,

Oliver

---

_Comment by @BurntSushi on 2020-05-15 12:20_

The [README says that a nightly Rust compiler is required to build ripgrep with the `simd-accel` feature](https://github.com/BurntSushi/ripgrep#building). Your error message says you are using a stable release, which is not nightly.

---

_Closed by @BurntSushi on 2020-05-15 12:20_

---

_Comment by @BurntSushi on 2020-05-15 12:22_

Note that unless you're searching non-UTF-8 encoded files, `simd-accel` isn't going to help with performance. Other uses of SIMD in ripgrep work with the Rust stable release and are automatically enabled at runtime if your CPU supports them. (The crate responsible for decoding non-UTF-8 files refuses to migrate to non-portable SIMD code, so it is stuck using the portable SIMD API which is only available using a nightly compiler.)

---

_Comment by @assarbad on 2020-05-27 07:28_

Thanks a bunch for the response and your time. And sorry for my late response. I'll try with the nightly then.

Just wanted to bring this to your attention. And yes, I simply ended up not using SIMD and it still was lightning fast.

---
