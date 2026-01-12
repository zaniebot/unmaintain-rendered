```yaml
number: 4833
title: Support workspace to workspace path dependencies
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: konsti/workspace-to-workspace-dependencies
created_at: 2024-07-05T11:52:00Z
updated_at: 2024-07-16T20:38:47Z
url: https://github.com/astral-sh/uv/pull/4833
synced_at: 2026-01-12T16:06:28Z
```

# Support workspace to workspace path dependencies

---

_@konstin_

Add support for path dependencies from a package in one workspace to a package in another workspace, which it self has workspace dependencies.

Say we have a main workspace with packages `a` and `b`, and a second workspace with `c` and `d`. We have `a -> b`, `b -> c`, `c -> d`. This would previously lead to a mangled path for `d`, which is now fixed.

Like distribution paths, we split workspace paths into an absolute install path and a relative (or absolute, if the user provided an absolute path) lock path.

Part of https://github.com/astral-sh/uv/issues/3943

---

_Label `bug` added by @konstin on 2024-07-07 20:17_

---

_Label `preview` added by @konstin on 2024-07-07 20:17_

---

_@konstin reviewed on 2024-07-07 20:19_

---

_Review comment by @konstin on `crates/uv-fs/src/path.rs`:216 on 2024-07-07 20:19_

This function is more lenient than the other path normalization function. Can/should we merge the two? I don't fully understand the constraints on the other normalization function, so i kept them apart for starters.

---

_@konstin reviewed on 2024-07-07 20:21_

---

_Review comment by @konstin on `crates/uv/tests/workspace.rs`:576 on 2024-07-07 20:21_

I'll reuse this for the other workspace-to-workspace cases

---

_Marked ready for review by @konstin on 2024-07-07 20:22_

---

_Review requested from @BurntSushi by @konstin on 2024-07-16 10:17_

---

_Review comment by @BurntSushi on `crates/uv-distribution/src/workspace.rs`:964 on 2024-07-16 16:05_

I would clarify here that the method _requires_ that the `path` is absolute, or else it panics. I think the current phrasing could imply that the given path is turned _into_ an absolute path by this function if it isn't already.

---

_Review comment by @BurntSushi on `crates/uv-fs/src/path.rs`:216 on 2024-07-16 16:08_

I don't fully understand the constraints either.

This function reminds me of [`std::path::absolute`](https://doc.rust-lang.org/std/path/fn.absolute.html). Should we use that instead here, or are the critical differences in semantics? (The phrasing in your docs of not touching the file system and possibly returning a path that does not exist were what reminded me of `absolute`.)

I typed all that up, but now I see that this routine is specifically handing relative paths and not making them absolute. So `absolute` is probably a no-go.

---

_Review comment by @BurntSushi on `crates/uv/tests/common/mod.rs`:878 on 2024-07-16 16:09_

Random question, but how we specify a build system here? Should I be doing this in my own ad hoc experiments?

---

_@BurntSushi approved on 2024-07-16 16:09_

Nice!

---

_@konstin reviewed on 2024-07-16 20:29_

---

_Review comment by @konstin on `crates/uv/tests/common/mod.rs`:878 on 2024-07-16 20:29_

If you specify nothing, we default to setuptools. Setuptools works, but has some quirks like creating an `.egg-info` directory and not checking if there's actual code for your package, so i normally go with hatch for my experiments:

```
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

Flit is another modern, more minimalistic build backend.

---

_@konstin reviewed on 2024-07-16 20:37_

---

_Review comment by @konstin on `crates/uv-fs/src/path.rs`:216 on 2024-07-16 20:37_

I found the difference:

> Note that no other normalization takes place; in particular, `a/c`
> and `a/b/../c` are distinct, to account for the possibility that `b`
> is a symbolic link (so its parent isn't `a`).

We assume they are the same and normalize this way.

---

_Merged by @konstin on 2024-07-16 20:38_

---

_Closed by @konstin on 2024-07-16 20:38_

---

_Branch deleted on 2024-07-16 20:38_

---
