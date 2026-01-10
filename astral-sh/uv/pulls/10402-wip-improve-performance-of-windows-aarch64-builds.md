```yaml
number: 10402
title: "WIP: Improve performance of windows-aarch64 builds"
type: pull_request
state: open
author: zanieb
labels:
  - internal
assignees: []
draft: true
base: main
head: zb/speedup-aarch64
created_at: 2025-01-08T17:48:16Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/10402
synced_at: 2026-01-10T11:10:34Z
```

# WIP: Improve performance of windows-aarch64 builds

---

_Pull request opened by @zanieb on 2025-01-08 17:48_

Extending #10306 — trying to reach an acceptable perf

---

_Label `internal` added by @zanieb on 2025-01-08 17:48_

---

_Renamed from "zb/speedup aarch64" to "WIP: Improve performance of windows-aarch64 builds" by @zanieb on 2025-01-08 17:48_

---

_Comment by @rich-ayr on 2025-01-09 01:25_

The "Install LLVM" step already installs LLVM/Clang 19 and puts it in path.
Visual Studio comes with Clang 18 which has a `bindgen` bug affecting `aarch64`, so I would advise not installing `Microsoft.VisualStudio.ComponentGroup.NativeDesktop.Llvm.Clang` unless you need Clang 18 specifically.

This will save some time on the installer as well, only getting the build tools and SDK takes around 5m 23s; although that shouldn't matter once you get the caching working 👍 

---

_Comment by @zanieb on 2025-01-09 02:53_

Interesting, thanks for the tips! 

---

_Comment by @zanieb on 2025-01-09 02:53_

Right now the problem is that the cache is _huge_, 7GB is going to cause cache churn since we have a 10GB limit in the repository.

---

_Comment by @rich-ayr on 2025-01-09 03:11_

> Right now the problem is that the cache is _huge_, 7GB is going to cause cache churn since we have a 10GB limit in the repository.

Oh wow even with caching it takes 3m 21s because of the download I assume.

I think it might be worth it to only use the native ARM64 runner in a release CI, Rust can use `--target aarch64...` just fine on `x86_64` hosts and this whole ">10 min CI just for getting a the bare minimum setup to compile rust" is kind of just burning money for no upside.

Probably best to wait for Microsoft/GitHub to ship a usable VM image.

---

_Comment by @zanieb on 2025-01-09 14:40_

> I think it might be worth it to only use the native ARM64 runner in a release CI, Rust can use --target aarch64... just fine on x86_64 hosts and this whole ">10 min CI just for getting a the bare minimum setup to compile rust" is kind of just burning money for no upside.

Yeah, but then it can regress and then I have to burn a bunch of cycles fixing a release :( trade-offs

> Probably best to wait for Microsoft/GitHub to ship a usable VM image.

Sort of agree.

---
