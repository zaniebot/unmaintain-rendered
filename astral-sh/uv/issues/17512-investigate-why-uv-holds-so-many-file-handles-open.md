```yaml
number: 17512
title: Investigate why uv holds so many file handles open
type: issue
state: open
author: konstin
labels:
  - bug
  - good first issue
  - help wanted
assignees: []
created_at: 2026-01-16T09:32:03Z
updated_at: 2026-01-16T09:32:03Z
url: https://github.com/astral-sh/uv/issues/17512
synced_at: 2026-01-16T09:55:15Z
```

# Investigate why uv holds so many file handles open

---

_@konstin_

We got reports that uv overruns the default open files limit (ulimit) for users on Linux and macOS:

* During sync/building packages: https://github.com/astral-sh/uv/issues/11296
* During bytecode compilation: https://github.com/astral-sh/uv/issues/16999
* When uninstall Python versions (no GitHub issue yet)

The default ulimits can be low, for example 1024 on linux, and we spawn up to a thread per core and several subprocesses. However, Cargo and other Rust tools which have very similar workloads and often very similar problems to us, don't have any problems with ulimits. A cargo maintainer [told us](https://rust-lang.zulipchat.com/#narrow/channel/246057-t-cargo/topic/Do.20cargo.2Frustc.20handle.20low.20ulimits.3F/near/568121917):

> From my testing for fine grain locking (unstable feature), I found that Cargo generally peaks with around ~70-80 file descriptors opened at once, and this remainly mostly static even for large projects.

So the question is, why are we running into the ulimit, while Cargo doesn't? What file descriptors is uv even holding, do we need them? Is Cargo doing something different that we can adopt to avoid this?

This requires some Unix expertise and rust parallelism knowledge, but should otherwise not require much uv specific details, this is mostly around figuring out how to analyze this this in uv and determining the differences to Cargo.

---

_Label `bug` added by @konstin on 2026-01-16 09:32_

---

_Label `good first issue` added by @konstin on 2026-01-16 09:32_

---

_Label `help wanted` added by @konstin on 2026-01-16 09:32_

---
