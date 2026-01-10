```yaml
number: 2651
title: "uv: add hidden --emit-marker-expression flag"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/emit-marker-expression
created_at: 2024-03-25T16:07:57Z
updated_at: 2024-03-27T15:59:47Z
url: https://github.com/astral-sh/uv/pull/2651
synced_at: 2026-01-10T14:49:08Z
```

# uv: add hidden --emit-marker-expression flag

---

_Pull request opened by @BurntSushi on 2024-03-25 16:07_

This PR adds a new flag, `--emit-marker-expression`, to `uv pip
compile`. The flag is hidden and not considered part of the stable API.

When given, this flag adjusts the output of `uv pip compile` to include
a comment that contains a marker expression. The guarantee provided
by this expression is that if it's true, then the pinned requirements
are guaranteed to be valid. (But there may be cases where the pinned
requirements are valid and the marker expression evaluates to false.)

This marker expression is meant to be one small piece of [Brett
Canon's lock file proposal]. Specifically, the `lock.markers` fields.
I implemented this to get an idea of what it would look like in the
context of `uv`.

The idea behind writing this into a comment is principally to make
this functionality easily testable via the CLI. It's not clear whether
this is something we'll want to make part of our stable CLI, but the
functionality it provides is likely something we will need in some form
eventually.

[Brett Canon's lock file proposal]: https://discuss.python.org/t/lock-files-again-but-this-time-w-sdists/46593


---

_Review requested from @charliermarsh by @BurntSushi on 2024-03-25 16:08_

---

_Review requested from @konstin by @BurntSushi on 2024-03-25 16:08_

---

_Label `internal` added by @zanieb on 2024-03-25 16:35_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolution.rs`:336 on 2024-03-26 16:36_

So what we do is collect all marker keys that are used and add them with the current platform, which is stricter than necessary (unlike add `sys_platform != "macos"`)?

---

_@konstin reviewed on 2024-03-26 16:43_

---

_Review comment by @konstin on `crates/uv/tests/pip_compile.rs`:6250 on 2024-03-26 16:43_

```suggestion
/// This tests the marker expressions emitted when depending on a package with 
a
```

---

_Review comment by @konstin on `crates/uv/tests/pip_compile.rs`:6257 on 2024-03-26 16:47_

Do you have reproduction instructions for this? That looks like a bug (https://inspector.pypi.io/project/pendulum/3.0.0/packages/67/5e/e646afbd1632bfbacdae79289d7d5879efdeeb5f5e58327bc5c698731107/pendulum-3.0.0-cp310-none-win_amd64.whl/pendulum-3.0.0.dist-info/METADATA#line.12)

---

_@konstin approved on 2024-03-26 16:47_

---

_@charliermarsh reviewed on 2024-03-26 18:11_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution.rs`:336 on 2024-03-26 18:11_

That's correct. This is necessary since the proposal only allows you to provide a dictionary of required markers, so we can't express `sys_platform != "macos"` -- only `sys_platform == "whatever_we_have"`.

---

_@charliermarsh reviewed on 2024-03-26 18:13_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_compile.rs`:453 on 2024-03-26 18:13_

I think this should be immediately after the `include_header`, and should also be green, since it's a comment. Some of the entries above this are _not_ comments and are actual content.

---

_@charliermarsh approved on 2024-03-26 18:14_

---

_@BurntSushi reviewed on 2024-03-27 14:49_

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_compile.rs`:6257 on 2024-03-27 14:49_

I think it is just `echo pendulum | uv pip compile .` on Windows. I only observed it on CI through this test though. I'll put up another PR with a regression test and see what happens.

---

_Merged by @BurntSushi on 2024-03-27 15:22_

---

_Closed by @BurntSushi on 2024-03-27 15:22_

---

_Branch deleted on 2024-03-27 15:22_

---

_@BurntSushi reviewed on 2024-03-27 15:59_

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_compile.rs`:6257 on 2024-03-27 15:59_

ref https://github.com/astral-sh/uv/pull/2693

---
