```yaml
number: 10629
title: "Patch embedded install path for Python dylib on macOS during `python install`"
type: pull_request
state: merged
author: LukeMathWalker
labels:
  - bug
assignees: []
merged: true
base: main
head: link-macos
created_at: 2025-01-15T11:02:26Z
updated_at: 2025-01-16T05:48:24Z
url: https://github.com/astral-sh/uv/pull/10629
synced_at: 2026-01-12T16:09:24Z
```

# Patch embedded install path for Python dylib on macOS during `python install`

---

_@LukeMathWalker_

## Summary

Fixes #10598 

## Test Plan

Looking for input here @zanieb. How/where would you include tests for this?
More broadly: do we want a failure to perform the rename to be a hard error? Or should it start out as a warning?


---

_Review requested from @zanieb by @konstin on 2025-01-15 11:38_

---

_Assigned to @zanieb by @zanieb on 2025-01-15 15:43_

---

_Review comment by @zanieb on `crates/uv-python/src/macos_dylib.rs`:20 on 2025-01-15 15:46_

Can we do this in a single invocation so we don't pay the overhead of invoking the tool twice? I think you should be able to match the error to determine if it failed because the command was missing.

---

_@zanieb reviewed on 2025-01-15 15:46_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:315 on 2025-01-15 15:50_

As you mentioned, this probably shouldn't be a hard error to start. We can turn it into a warning here, or in the called function. Here seems a bit more ergonomic because in a future change, we could push it to a `warnings` list like we do for `errors`.

---

_@zanieb reviewed on 2025-01-15 15:50_

---

_@zanieb reviewed on 2025-01-15 15:51_

---

_Review comment by @zanieb on `crates/uv-python/src/macos_dylib.rs`:44 on 2025-01-15 15:51_

This is not used right now, right?

I like the message but I think it'd be overwhelming to beginners (who probably don't care if this failed?).

---

_Comment by @zanieb on 2025-01-15 16:00_

Nice patch! I guess we can add a macOS-only test case for this in `crates/uv/tests/it/python_install.rs` and validate the output of otool? I'm not sure if we want to just snapshot the otool output or assert the expected substring is present / the other is not. Sometimes both is best.

---

_Label `bug` added by @zanieb on 2025-01-15 16:01_

---

_Renamed from "Fix: path install name for Python's dylibs on macOS when the toolchain is installed" to "Patch embedded install path for Python dylib on macOS during `python install`" by @zanieb on 2025-01-15 16:02_

---

_Review comment by @LukeMathWalker on `crates/uv/src/commands/python/install.rs`:315 on 2025-01-15 16:15_

I've done this in [95d3adf](https://github.com/astral-sh/uv/pull/10629/commits/95d3adf851f75b53b5b52f35aee27e5e685dbe8a)
There's a bit of duplication with respect to the warning message, but since it's just two callsite it's probably acceptable. Happy to move it into the function itself if you prefer.

---

_@LukeMathWalker reviewed on 2025-01-15 16:15_

---

_Review comment by @LukeMathWalker on `crates/uv-python/src/macos_dylib.rs`:20 on 2025-01-15 16:15_

Yes we can! Done in [95d3adf](https://github.com/astral-sh/uv/pull/10629/commits/95d3adf851f75b53b5b52f35aee27e5e685dbe8a)

---

_@LukeMathWalker reviewed on 2025-01-15 16:15_

---

_Review comment by @LukeMathWalker on `crates/uv-python/src/macos_dylib.rs`:44 on 2025-01-15 16:16_

That was an oversight, it should have been returned on if the first command errored out.
I've streamlined the error message in [95d3adf](https://github.com/astral-sh/uv/pull/10629/commits/95d3adf851f75b53b5b52f35aee27e5e685dbe8a), and it will only be shown if logs are enabled.
Do you think what I used for `warn_user` is acceptable in terms of level of details and verbosity?

---

_@LukeMathWalker reviewed on 2025-01-15 16:16_

---

_@zanieb reviewed on 2025-01-15 16:23_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:179 on 2025-01-15 16:23_

This seems reasonable, I like the messaging. You probably don't want to repeat the message. You could include `e` in the warning conditional on `tracing::enabled!(tracing::Level::DEBUG)` but I think it's also reasonable to just `debug!("{}", e)` and leave the warning as-is.

---

_@LukeMathWalker reviewed on 2025-01-15 16:51_

---

_Review comment by @LukeMathWalker on `crates/uv-python/src/installation.rs`:179 on 2025-01-15 16:51_

That's a good point, I have done it in https://github.com/astral-sh/uv/pull/10629/commits/200698f801097e0b8e2079a79d10e6bb818d5623

---

_@LukeMathWalker reviewed on 2025-01-15 16:53_

---

_Review comment by @LukeMathWalker on `crates/uv/tests/it/python_install.rs`:898 on 2025-01-15 16:53_

This bit is not going to fly, since it may run on multiple platforms, not just "aarch64-none". 
I haven't found a way to get the "current" platform tag though. Any pointers? @zanieb 

---

_@zanieb reviewed on 2025-01-15 17:40_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:898 on 2025-01-15 17:40_

Perhaps `platform_key_from_env`

---

_@zanieb approved on 2025-01-15 19:28_

---

_@zanieb reviewed on 2025-01-15 19:28_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:898 on 2025-01-15 19:28_

I did this in https://github.com/astral-sh/uv/pull/10629/commits/4f2dfbabe3118fd982d20da32a08e9844d2729e0 quick so we can release it today.

---

_Comment by @zanieb on 2025-01-15 19:28_

Thanks for taking the time to contribute and address the feedback — solid work :)

---

_Merged by @zanieb on 2025-01-15 20:11_

---

_Closed by @zanieb on 2025-01-15 20:11_

---

_Review comment by @LukeMathWalker on `crates/uv/tests/it/python_install.rs`:898 on 2025-01-16 05:48_

Thank you!

---

_@LukeMathWalker reviewed on 2025-01-16 05:48_

---
