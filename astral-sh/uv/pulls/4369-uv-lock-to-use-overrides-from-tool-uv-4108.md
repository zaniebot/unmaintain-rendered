```yaml
number: 4369
title: "uv lock to use overrides from tool.uv (#4108)"
type: pull_request
state: merged
author: idlsoft
labels:
  - needs-design
assignees: []
merged: true
base: main
head: lock_with_overrides
created_at: 2024-06-17T20:16:37Z
updated_at: 2024-06-24T16:44:55Z
url: https://github.com/astral-sh/uv/pull/4369
synced_at: 2026-01-12T16:06:11Z
```

# uv lock to use overrides from tool.uv (#4108)

---

_@idlsoft_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This will make `uv lock` read `override-dependencies` from  the `[tool.uv]` section of `pyproject.toml`.
Resolves #4108

This [other](https://github.com/astral-sh/uv/pull/4446)  implementation touches more code but seems more consistent.

## Test Plan

Unit test

---

_Marked ready for review by @idlsoft on 2024-06-17 21:23_

---

_Review requested from @BurntSushi by @zanieb on 2024-06-18 23:52_

---

_Review requested from @ibraheemdev by @zanieb on 2024-06-18 23:52_

---

_Review comment by @BurntSushi on `crates/uv/src/commands/project/lock.rs`:146 on 2024-06-19 13:27_

I think this can be removed?

---

_@BurntSushi approved on 2024-06-19 13:29_

I think this LGTM, but I think @konstin should sign off on this before merging.

---

_Review requested from @konstin by @BurntSushi on 2024-06-19 13:29_

---

_Comment by @konstin on 2024-06-19 14:46_

Currently, this can have effects on neighbouring packages: Say you have a non-project workspace root and in it two package foo, bar and baz.

foo has:
```toml
tool.uv.override-dependencies = [
   "anyio==4.3.0"
]
```

bar has:
```toml
dependencies = [
    "anyio==4.4.0"
]
```

baz has:
```toml
tool.uv.override-dependencies = [
   "lib_that_uses_anyio=1.2.3"
]
```

This means foo changes the resolution of both bar and baz, once directly and once indirectly. Since overrides apply globally, i think we should only allow them in the workspace root.

---

_Label `needs-design` added by @konstin on 2024-06-19 14:47_

---

_@idlsoft reviewed on 2024-06-19 15:08_

---

_Review comment by @idlsoft on `crates/uv/src/commands/project/lock.rs`:146 on 2024-06-19 15:08_

> I think this can be removed?

Wanted to make it explicit that it's now immutable

---

_@BurntSushi reviewed on 2024-06-19 15:11_

---

_Review comment by @BurntSushi on `crates/uv/src/commands/project/lock.rs`:146 on 2024-06-19 15:11_

We don't usually do this in our code. It would add a lot of noise.

---

_@konstin reviewed on 2024-06-19 15:13_

---

_Review comment by @konstin on `crates/uv/src/commands/project/lock.rs`:146 on 2024-06-19 15:13_

fyi i removed it and also changed the iteration style to align more with what we do elsewhere

---

_Comment by @idlsoft on 2024-06-19 16:01_

> This means foo changes the resolution of both bar and baz, once directly and once indirectly. Since overrides apply globally, i think we should only allow them in the workspace root.

I assume you mean `foo`, `bar` and `baz` don't depend on each other.
You're right, but it's also not entirely new. The fact that one virtualenv is shared by all workspace members is bound to have side-effects.

As convinient as a shared virtualenv is, I kinda wish there was a way to require a private one per member.
As it stands right now dependency versions are negotiated by unrelated workspace members.
Also you can have a line in `bar` that references `lib_that_uses_anyio` or `foo`, and it'll work fine in the virtualenv, but not when `bar` is shipped as a standalone library. 

On an unrelated note, if someone could also take a look at https://github.com/astral-sh/rye/pull/1015 I'd appreciate it. I'm a bit stuck with one of my projects.

---

_Comment by @idlsoft on 2024-06-21 23:27_

I've tried a [different implementation](https://github.com/astral-sh/uv/pull/4446), I think it's more in line with existing code.

---

_Comment by @idlsoft on 2024-06-24 15:03_

Updated, it now gets overrides from the workspace only.

---

_Comment by @charliermarsh on 2024-06-24 16:44_

I think this looks correct.

---

_Merged by @charliermarsh on 2024-06-24 16:44_

---

_Closed by @charliermarsh on 2024-06-24 16:44_

---
