```yaml
number: 9130
title: add support for package level conflicts with groups
type: pull_request
state: closed
author: BurntSushi
labels:
  - resolver
assignees: []
base: main
head: ag/package-level-conflict
created_at: 2024-11-14T19:35:12Z
updated_at: 2025-07-25T20:39:05Z
url: https://github.com/astral-sh/uv/pull/9130
synced_at: 2026-01-12T16:08:40Z
```

# add support for package level conflicts with groups

---

_@BurntSushi_

This PR adds support for declaring a conflict between a group
and a project's "production" dependencies. For example:

```toml
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "anyio>=4.6.2.post1",
]

[dependency-groups]
foo = [
    "anyio<4.6.2.post1",
]

[tool.uv]
conflicts = [
  [
    { group = "foo" },
    { package = "example" },
  ],
]
```

Before this PR, syntax like `{ package = "example" }` wasn't allowed.
This PR enables that syntax, and also tweaks the forking logic a bit
to support this.

I don't have 100% confidence in this PR. In particular, I'm not sure
if my trick adding a parent package to `PubGrubDependency` is fully
correct. But I think it works.


---

_Review requested from @charliermarsh by @BurntSushi on 2024-11-14 19:37_

---

_Review requested from @zanieb by @BurntSushi on 2024-11-14 19:37_

---

_Review requested from @konstin by @BurntSushi on 2024-11-14 19:37_

---

_@konstin approved on 2024-11-15 10:12_

---

_Comment by @zanieb on 2024-11-16 16:06_

(I think the experience makes sense, not feeling confident about reviewing the implementation)

---

_Label `resolver` added by @BurntSushi on 2024-11-23 15:59_

---

_Comment by @zanieb on 2025-07-10 19:17_

I've done a very dubious update with `main`.

---

_Assigned to @zanieb by @zanieb on 2025-07-18 11:47_

---

_Comment by @zanieb on 2025-07-22 19:37_

@BurntSushi does this also enable package <-> package conflicts? I'd expect that for workspace members. I think the use-case there is more important than package <-> group conflicts. (I can investigate this too, we'll want test coverage)

---

_Comment by @BurntSushi on 2025-07-23 15:26_

Hmmm with this updated, the example in the PR description now generates a bunk `uv.lock`:

```toml
version = 1
revision = 2
requires-python = ">=3.12"
resolution-markers = [
    "python_version < '0'",
]
conflicts = [[
    { package = "example", group = "foo" },
    { package = "example" },
]]
```

Specifically, the `python_version < '0'` marker.

I'll take a closer look.

---

_Comment by @BurntSushi on 2025-07-23 18:08_

OK, I've pushed up a commit that I think gets this PR into working shape. There was a fair bit missing from `UniversalMarker` in sections that I think came after this PR was originally written. There was also a test asserting incorrect results (in the same way that the example in the PR description failed).

> @BurntSushi does this also enable package <-> package conflicts? I'd expect that for workspace members. I think the use-case there is more important than package <-> group conflicts. (I can investigate this too, we'll want test coverage)

I think all the bones for this are there, but it doesn't seem to be fully working. Here's a simple concrete example. The root `pyproject.toml`:

```toml
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["sortedcontainers==2.3.0"]

[tool.uv.workspace]
members = ["subexample"]

[tool.uv]
conflicts = [
  [
    { package = "example" },
    { package = "subexample" },
  ],
]

[build-system]
requires = ["setuptools>=42"]
build-backend = "setuptools.build_meta"
```

And now `subexample/pyproject.toml`:

```toml
[project]
name = "subexample"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["sortedcontainers==2.4.0"]
```

Without the conflicts declared here, this would normally fail to generate a lock file. But _that_ part works and seems to generate a correct lock. But syncing fails (ignore the bad error messages for now):

```
$ run-uv -q pr1 -- sync
Resolved 4 packages in 5ms
error: The project and the project are incompatible with the declared conflicts: {the project, the project}

$ run-uv -q pr1 -- sync --package example
Resolved 4 packages in 3ms
error: The project and the project are incompatible with the declared conflicts: {the project, the project}

$ run-uv -q pr1 -- sync --package subexample
Resolved 4 packages in 6ms
error: The project and the project are incompatible with the declared conflicts: {the project, the project}
```

In order for these package level conflicts to work, there has to be way to sync only a specific package or set of packages.


---

_Comment by @konstin on 2025-07-23 20:44_

(I'm still tagged here from the previous review, let me know if you want me to do another round)

---

_Comment by @zanieb on 2025-07-23 22:18_

Thank you for addressing the problems!

>  there has to be way to sync only a specific package or set of packages.

We do already sync a subset, e.g., I don't think `subexample` would be installed there without `--all-packages` since it's not a dependency? I'll look into this.

---

_@zanieb reviewed on 2025-07-24 22:24_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock_conflict.rs`:1052 on 2025-07-24 22:24_

Just insta version churn / noise in this file.

---

_Comment by @zanieb on 2025-07-25 20:02_

I'm going to move this to another pull request since it's got a pretty different scope now and it's a pain to keep looking up Andrew's PRs instead of my own :)

See https://github.com/astral-sh/uv/pull/14906

---

_Closed by @zanieb on 2025-07-25 20:02_

---
