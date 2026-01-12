```yaml
number: 14369
title: Add UV_COMPILE_BYTECODE_TIMEOUT environment variable
type: pull_request
state: merged
author: adisbladis
labels:
  - configuration
assignees: []
merged: true
base: main
head: configurable-bytecode-timeout
created_at: 2025-06-30T09:01:29Z
updated_at: 2025-07-17T13:11:33Z
url: https://github.com/astral-sh/uv/pull/14369
synced_at: 2026-01-12T16:11:11Z
```

# Add UV_COMPILE_BYTECODE_TIMEOUT environment variable

---

_@adisbladis_

## Summary

When installing packages on _very_ slow/overloaded systems it'spossible to trigger bytecode compilation timeouts, which tends to happen in environments such as Qemu (especially without KVM/virtio), but also on systems that are simply overloaded. I've seen this in my Nix builds if I for example am compiling a Linux kernel at the same time as a few other concurrent builds.

By making the bytecode compilation timeout adjustable you can work around such issues. I plan to set `UV_COMPILE_BYTECODE_TIMEOUT=0` in the [pyproject.nix builders](https://pyproject-nix.github.io/pyproject.nix/build.html) to make them more reliable.

- Related issues

  * https://github.com/astral-sh/uv/issues/6105

## Test Plan

Only manual testing was applied in this instance. There is no existing automated tests for bytecode compilation timeout afaict.

---

_Comment by @konstin on 2025-06-30 09:34_

Are you sure you're hitting the timeout and not the QEMU deadlock? If so, can you share a reproduction for it?

---

_Comment by @adisbladis on 2025-06-30 11:17_

> Are you sure you're hitting the timeout and not the QEMU deadlock?

I was hitting this earlier when building a NixOS system on a laptop, no QEMU involved.
It was building a Linux kernel concurrently with a few other Nix packages, one of which was compiling Python bytecode using uv.

> If so, can you share a reproduction for it?

It's really hard to provide a reproduction because it only happens under very high load, and even then it's far from reliable.
I've mostly observed it when I was having very high load due to many concurrent Nix builds & was thermally throttled.

---

_Comment by @konstin on 2025-07-01 10:35_

It's hard for us to justify adding a new flag with without a way for to reproduce why we need and to compare it with other solutions, e.g. raising the fixed timeout.

---

_@zanieb approved on 2025-07-11 17:26_

I don't think we should have timeouts that can't be configured, generally speaking. I don't mind supporting this — if it's a deadlock then changing the timeout won't fix anything and we'll have learned something.

---

_Label `configuration` added by @zanieb on 2025-07-11 17:28_

---

_@zanieb reviewed on 2025-07-11 17:54_

---

_Review comment by @zanieb on `crates/uv-installer/src/compile.rs`:102 on 2025-07-11 17:54_

We should probably error here though, I don't think we have much precedent for warning on invalid options.

---

_@adisbladis reviewed on 2025-07-16 00:54_

---

_Review comment by @adisbladis on `crates/uv-installer/src/compile.rs`:102 on 2025-07-16 00:54_

I copied this from the handling of `UV_HTTP_TIMEOUT`/`UV_REQUEST_TIMEOUT`/`HTTP_TIMEOUT`.
That might have been handled differently because of `HTTP_TIMEOUT` being more generic?

In any case I agree with erroring out on an invalid value.

---

_@zanieb reviewed on 2025-07-16 14:03_

---

_Review comment by @zanieb on `crates/uv-installer/src/compile.rs`:99 on 2025-07-16 14:03_

Sorry I wasn't clear — we shouldn't panic here, we try to avoid panicking in general. We should return this as a Result::Err variant.

---

_Review comment by @adisbladis on `crates/uv-installer/src/compile.rs`:99 on 2025-07-17 02:44_

Ok. I've introduced a `CompileError::EnvironmentError`.

---

_@adisbladis reviewed on 2025-07-17 02:44_

---

_@zanieb approved on 2025-07-17 13:11_

---

_Merged by @zanieb on 2025-07-17 13:11_

---

_Closed by @zanieb on 2025-07-17 13:11_

---
