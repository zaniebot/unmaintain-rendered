```yaml
number: 10375
title: Upgrade zlib-ng
type: pull_request
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
base: main
head: charlie/zlib-ng
created_at: 2025-01-07T18:25:59Z
updated_at: 2025-01-09T14:26:48Z
url: https://github.com/astral-sh/uv/pull/10375
synced_at: 2026-01-10T11:44:45Z
```

# Upgrade zlib-ng

---

_Pull request opened by @charliermarsh on 2025-01-07 18:25_

## Summary

If we remove this static linking, we can upgrade `zlib-ng`, which in turn lets us enable it on PowerPC.

---

_Label `performance` added by @charliermarsh on 2025-01-07 18:30_

---

_Marked ready for review by @charliermarsh on 2025-01-07 18:30_

---

_@charliermarsh reviewed on 2025-01-07 18:50_

---

_Review comment by @charliermarsh on `.cargo/config.toml`:9 on 2025-01-07 18:50_

We did add this for a reason (https://github.com/astral-sh/ruff/issues/11503) so we need to decide if it's important to retain.

---

_@T-256 reviewed on 2025-01-07 20:14_

---

_Review comment by @T-256 on `.cargo/config.toml`:9 on 2025-01-07 20:14_

Do you mean root cause of that issue was in `zlib-ng`?

---

_Review requested from @Gankra by @zanieb on 2025-01-09 00:32_

---

_Review requested from @konstin by @zanieb on 2025-01-09 00:32_

---

_Comment by @musicinmybrain on 2025-01-09 00:43_

If you don’t end up merging this for some reason, please consider at least making the version bound `libz-ng-sys = { version = "<1.1.20" }` Windows-specific. (The comment above it references a Windows-specific issue, https://github.com/rust-lang/libz-sys/issues/225.) Otherwise, I have to patch out the version bound in Fedora in order to use the system `rust-libz-ng-sys-1.1.20` package, which is a minor but unnecessary inconvenience.

For now, I’ll just watch this PR, since it would remove the `libz-ng-sys` version bound altogether.

---

_Comment by @charliermarsh on 2025-01-09 00:57_

I tried that, but you actually can't make it Windows-specific. Cargo doesn't let you use multiple dependencies when linking a native library, apparently:

```
error: failed to select a version for `libz-ng-sys`.
    ... required by package `uv-performance-flate2-backend v0.1.0 (/Users/crmarsh/workspace/puffin/crates/uv-performance-flate2-backend)`
versions that meet the requirements `<1.1.20` are: 1.1.16, 1.1.15, 1.1.14, 1.1.13, 1.1.12, 1.1.11, 1.1.10, 1.1.9, 1.1.8, 1.1.7

the package `libz-ng-sys` links to the native library `z-ng`, but it conflicts with a previous package which links to `z-ng` as well:
package `libz-ng-sys v1.1.20`
    ... which satisfies dependency `libz-ng-sys = ">=1.1.20"` of package `uv-performance-flate2-backend v0.1.0 (/Users/crmarsh/workspace/puffin/crates/uv-performance-flate2-backend)`
Only one package in the dependency graph may specify the same links value. This helps ensure that only one copy of a native library is linked in the final binary. Try to adjust your dependencies so that only one package uses the `links = "z-ng"` value. For more information, see https://doc.rust-lang.org/cargo/reference/resolver.html#links.
```

---

_Comment by @musicinmybrain on 2025-01-09 11:39_

> I tried that, but you actually can't make it Windows-specific. Cargo doesn't let you use multiple dependencies when linking a native library, apparently:

That kind of almost makes sense – mostly – in retrospect… I think… possibly. Anyway, thanks for attempting it!

---

_Review comment by @Gankra on `.cargo/config.toml`:9 on 2025-01-09 14:14_

No the issue is fundamental to windows and how it deals with ~system libraries. At least one thing we would regard as equivalent to "glibc" on linux is not shipped by default on windows (VCRUNTIME). When you ship a Windows application you are either expected to include an installer ("redistributable") for the version of VCRUNTIME that you need and invoke it to tell windows to add it to the system, *or*, you're expected to statically link it.

The above config chooses the latter, making us more statically linked in the same way that statically linking musl does. This is good for installing our binaries on any random Windows machine without additional installation stuff.

However it has a similar side-effect that musl does: if you need to dynamically link *any* other libraries you will likely get errors, because (*handwaving*) the other libraries dynamically link to VCRUNTIME-et-al and treat the contents of VCRUNTIME as protocol you need to interoperate with.

zlib-ng failing to link properly when we have this setting enabled is indicative of *it* wanting to dynamically link extra things, and running into the fact that we've essentially opted into "no you don't". (I haven't verified this but it's the only thing that makes sense to me).

By deleting this config (and doing nothing else), we are essentially opting into a lottery where we *really* hope all our users happened to have installed an appropriate version of VCRUNTIME before they install uv. This was the original issue ruff ran into. So we shouldn't remove it without a solution for redistributables (or research indicating this is just guaranteed to be on all supported systems now).

---

_@Gankra requested changes on 2025-01-09 14:14_

---

_Comment by @charliermarsh on 2025-01-09 14:26_

Closing based on @Gankra's great diagnosis.

---

_Closed by @charliermarsh on 2025-01-09 14:26_

---
