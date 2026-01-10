```yaml
number: 2615
title: Avoid ignore crate finding cache gitignore
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/gitignore-on-the-root
created_at: 2024-03-22T15:42:51Z
updated_at: 2024-03-22T19:36:07Z
url: https://github.com/astral-sh/uv/pull/2615
synced_at: 2026-01-10T14:49:08Z
```

# Avoid ignore crate finding cache gitignore

---

_Pull request opened by @konstin on 2024-03-22 15:42_

We put a `.gitignore` with `*` at the top of our cache. When maturin was building a source distribution inside the cache, it would walk up the tree to find a gitignore, see that and ignore all python files. We now add an (empty) `.git` directory one directory below, in the root of built-wheels cache. This prevents ignore walking further up (it marks the top level a git repository).

Deptry (from #2490) is a mid sized rust package with additional python packages, so instead of using it in the test i've replaced it with a small (44KB total) reproducer that uses cffi for faster building, the entire test taking <2s on my machine.

Fixes #2490

---

_Label `bug` added by @konstin on 2024-03-22 15:42_

---

_Review requested from @BurntSushi by @konstin on 2024-03-22 15:42_

---

_Review requested from @charliermarsh by @konstin on 2024-03-22 15:42_

---

_@BurntSushi approved on 2024-03-22 15:57_

---

_Comment by @zanieb on 2024-03-22 15:57_

:D this is an endless source of trouble

Related:

- #2466 
- https://github.com/pypa/hatch/issues/1273

---

_Comment by @zanieb on 2024-03-22 15:58_

Why doesn't #2466 address this?

---

_Comment by @konstin on 2024-03-22 16:03_

As i understand it, ignore walks up until it finds a git repository root, since gitignores extend each other (cc @BurntSushi)

---

_Comment by @BurntSushi on 2024-03-22 16:34_

> As i understand it, ignore walks up until it finds a git repository root, since gitignores extend each other (cc @BurntSushi)

Yeah, although you can opt out of `.git` detection with [`WalkBuilder::require_git`](https://docs.rs/ignore/latest/ignore/struct.WalkBuilder.html#method.require_git).

You can also explicitly opt out of the "ascend the directory tree" behavior with [`WalkBuilder::parents`](https://docs.rs/ignore/latest/ignore/struct.WalkBuilder.html#method.parents). (@konstin I forgot to ask whether using this would be appropriate in this context.)

---

_@charliermarsh approved on 2024-03-22 17:17_

---

_@charliermarsh reviewed on 2024-03-22 17:17_

---

_Review comment by @charliermarsh on `scripts/packages/deptry_reproducer/Cargo.toml`:2 on 2024-03-22 17:17_

Can we put this in `editable-installs`, just because that's where all our other local packages are?

---

_@konstin reviewed on 2024-03-22 18:02_

---

_Review comment by @konstin on `scripts/packages/deptry_reproducer/Cargo.toml`:2 on 2024-03-22 18:02_

Do you mind if i rename editable installs to `packages` first in a separate PR?

---

_@charliermarsh reviewed on 2024-03-22 18:08_

---

_Review comment by @charliermarsh on `scripts/packages/deptry_reproducer/Cargo.toml`:2 on 2024-03-22 18:08_

Yeah, that’s fine.

---

_Comment by @zanieb on 2024-03-22 19:20_

@charliermarsh should I release this?

---

_Comment by @charliermarsh on 2024-03-22 19:33_

@zanieb - Yeah, let's include it. @konstin can rename the directory as a follow-up instead of a precursor PR.

---

_Merged by @zanieb on 2024-03-22 19:36_

---

_Closed by @zanieb on 2024-03-22 19:36_

---

_Branch deleted on 2024-03-22 19:36_

---
