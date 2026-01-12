```yaml
number: 8517
title: "Prefer `x86_64_v2` Python builds"
type: pull_request
state: closed
author: j178
labels:
  - uv python
assignees: []
base: main
head: x86_64_v2
created_at: 2024-10-24T06:50:37Z
updated_at: 2024-12-10T18:26:45Z
url: https://github.com/astral-sh/uv/pull/8517
synced_at: 2026-01-12T16:08:21Z
```

# Prefer `x86_64_v2` Python builds

---

_@j178_

## Summary

Resolves #8499



---

_Marked ready for review by @j178 on 2024-10-24 15:04_

---

_Review comment by @konstin on `crates/uv-python/fetch-download-metadata.py`:413 on 2024-10-24 16:09_

Nit: This can be a method on `Arch`

---

_@konstin approved on 2024-10-24 16:12_

Thank you!

---

_Review requested from @zanieb by @konstin on 2024-10-24 16:12_

---

_Comment by @konstin on 2024-10-24 16:13_

I check that the versions uv uses all run on my machine.

CC @zanieb for the changes in the generator script.

---

_Comment by @zanieb on 2024-10-24 16:17_

Can we use these unconditionally? Don't we need to check if the system supports the v2 instruction set before selecting these distributions? 

---

_Comment by @konstin on 2024-10-24 16:28_

According to https://gregoryszorc.com/docs/python-build-standalone/main/running.html:

> x86_64_v2-*
>
> Targets 64-bit Intel/AMD CPUs approximately newer than [Nehalem](https://en.wikipedia.org/wiki/Nehalem_(microarchitecture)) (released in 2008).
> Binaries will have SSE3, SSE4, and other CPU instructions added after the ~initial x86-64 CPUs were launched in 2003.
> Binaries will crash if you attempt to run them on an older CPU not supporting the newer instructions.

Requiring at least a cpu from 2008 is safe enough. I'd like to add cpu feature detection on top of that (e.g. avx2 for x86_64_v3), but this change in itself is a good idea.

---

_Comment by @zanieb on 2024-10-24 16:31_

I think we should probably retain the `x86_64` downloads (so you can install them by key) then add and prefer the `x86_64_v2` downloads by default. Removing access to the `x86_64` downloads entirely doesn't make sense to me.

Also, this is a breaking change; although it may not be likely. We might want to roll it into 0.5.0.

---

_Label `uv python` added by @zanieb on 2024-10-24 16:31_

---

_Comment by @zanieb on 2024-10-24 16:33_

I'm not particularly enthused about defaulting to the v2 builds without checking the CPU features, especially since we need to do so anyway in the future. But I feel less strongly about that as long as there's a path towards recovering the existing behavior.

---

_Comment by @danielhollas on 2024-10-24 21:33_

Are there any benchmarks that show whether / how much CPython is faster with these extra instructions? Might by interesting to run the pyperformance test suite (which is what faster CPython team uses for benchmarking).

---

_Comment by @notatallshaw-gts on 2024-11-13 15:47_

Is there a way to manually set uv to prefer v2 or v3 builds?

And then this could be documented as a non-default performance enhancement. 

---

_Comment by @charliermarsh on 2024-12-04 01:41_

I might finish this up, it looks straightforward to detect this with https://doc.rust-lang.org/std/macro.is_x86_feature_detected.html.

---

_Closed by @charliermarsh on 2024-12-04 01:41_

---

_Reopened by @charliermarsh on 2024-12-04 01:41_

---

_Comment by @charliermarsh on 2024-12-04 01:45_

(I didn't mean to close it, accidental click.)

---

_Comment by @zanieb on 2024-12-10 18:26_

Taking this up in https://github.com/astral-sh/uv/pull/9781

---

_Closed by @zanieb on 2024-12-10 18:26_

---
