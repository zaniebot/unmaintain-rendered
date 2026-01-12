```yaml
number: 9788
title: Detect x86_64 microarchitecture variant automatically
type: pull_request
state: open
author: zanieb
labels:
  - enhancement
assignees: []
draft: true
base: main
head: zb/micro-detect
created_at: 2024-12-10T21:01:08Z
updated_at: 2025-04-21T22:34:31Z
url: https://github.com/astral-sh/uv/pull/9788
synced_at: 2026-01-12T16:08:59Z
```

# Detect x86_64 microarchitecture variant automatically

---

_@zanieb_

e.g., on my test machine

```
❯ cargo run -- python install 3.12
Installed Python 3.12.8 in 3.65s
 + cpython-3.12.8-linux-x86_64_v3-gnu
```

Note this is split into two commits:

- [c0f146c](https://github.com/astral-sh/uv/pull/9788/commits/c0f146cc905a9dc93582c9f03bf9b4eaf3c9f426): which adds detection
- [5d78bf2](https://github.com/astral-sh/uv/pull/9788/commits/5d78bf2fedc8af65327f48be882268acf7d565f0): which improves behaviors and compatibility between architecture variants


---

_Label `enhancement` added by @zanieb on 2024-12-10 21:01_

---

_@zanieb reviewed on 2024-12-10 21:04_

---

_Review comment by @zanieb on `crates/uv-python/src/platform.rs`:160 on 2024-12-10 21:04_

tbh no clue if we should check for all of these or not

---

_Comment by @notatallshaw-gts on 2024-12-10 22:13_

While mostly I would want uv to automatically detect which microarchitecture variant to grab, I have a concern here related to docker images, specifically if uv is used to install Python in the dockerfile, what happens when the build machine and execution machine support different instruction sets?

---

_Comment by @zanieb on 2024-12-10 22:18_

> I have a concern here related to docker images, specifically if uv is used to install Python in the dockerfile, what happens when the build machine and execution machine support different instruction sets?

I think it's fair to say the build machine will need to be on the same (or an older) microarchitecture than your execution machine. Do you think that's unreasonable?

---

_Comment by @notatallshaw-gts on 2024-12-10 22:20_

> I think it's fair to say the build machine will need to be on the same (or an older) microarchitecture than your execution machine. Do you think that's unreasonable?

Only in the sense it's not something I have to think about right now, and without being familiar with this I'm not sure it would be easy to diagnose when a failure happened?

---

_Comment by @zanieb on 2024-12-10 22:21_

> Only in the sense it's not something I have to think about right now, and without being familiar with this I'm not sure it would be easy to diagnose when a failure happened?

Yeah the interpreter would probably just crash at runtime, or, if you use uv as your entry point, it would be "missing"

---

_Comment by @notatallshaw-gts on 2024-12-10 22:22_

Maybe this is already an issue with other tools that are installed into docker images, and it's not that common to face, and people who do face this problem are familiar with the symptom and cause. I just never thought about it before now.

---

_Comment by @danielhollas on 2024-12-11 13:45_

> Maybe this is already an issue with other tools that are installed into docker images, and it's not that common to face, and people who do face this problem are familiar with the symptom and cause. I just never thought about it before now.

I don't think this is a common issue right now, and you raised a very valid concern. For pre-built binary artifacts, for example most Linux distros AFAIK use conservative microarchitecture and don't do any runtime detection. 

There has been some discussion about this in Fedora but the proposal to bump microarchutecture has been withdrawn?

https://discussion.fedoraproject.org/t/will-future-releases-require-x86-64-v3/131200/6

---

_Comment by @drmikehenry on 2024-12-28 12:06_

Assuming this feature is eventually implemented, it would be helpful to have a way to disable CPU microarchitecture detection to retain the current behavior that v1 is used by default.  This would make it possible for folks who are mirroring `python-build-standalone` to continue to mirror only the v1 binaries rather than the whole set of microarchitectures, reducing both space and time requirements for mirroring.  Developers might also prefer to use the same Python executables across an entire team for consistency.


---

_Comment by @zanieb on 2024-12-28 17:18_

@drmikehenry thanks for the feedback!

---

_@uwu-420 reviewed on 2025-03-01 18:25_

---

_Review comment by @uwu-420 on `crates/uv-python/src/platform.rs`:160 on 2025-03-01 18:25_

All those checks are necessary I think.

I'd even go further and do it similar to https://github.com/HenrikBengtsson/x86-64-level (fyi, it takes the values from `/proc/cpuinfo` so the names slightly differ)
```bash
determine_cpu_version() {
    ## x86-64-v0 (can this happen?)
    level=0
    
    ## x86-64-v1
    has_cpu_flags lm cmov cx8 fpu fxsr mmx syscall sse2 || return 0
    level=1

    ## x86-64-v2
    has_cpu_flags cx16 lahf_lm popcnt sse4_1 sse4_2 ssse3 || return 0
    level=2
    
    ## x86-64-v3
    has_cpu_flags avx avx2 bmi1 bmi2 f16c fma abm movbe xsave || return 0
    level=3

    ## x86-64-v4
    has_cpu_flags avx512f avx512bw avx512cd avx512dq avx512vl || return 0
    level=4
}
```
I think going bottom-up like this is more robust although not really necessary in practice I guess. A CPU supporting AVX512 will support AVX2 😃 Unless someone is running uv in a crazy emulated environment. Nevertheless, it could be a good idea to comment that each level is a superset of the level below and that in practice it's okay to check top-down instead of bottom-up.

But that `abm` feature is confusing me, I couldn't find it directly mentioned in these sources
- https://en.wikipedia.org/wiki/X86-64#Microarchitecture_levels
- https://gitlab.com/x86-psABIs/x86-64-ABI/-/blob/master/x86-64-ABI/low-level-sys-info.tex
- https://clang.llvm.org/docs/UsersManual.html#x86

---
