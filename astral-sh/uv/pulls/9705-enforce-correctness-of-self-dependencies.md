```yaml
number: 9705
title: Enforce correctness of self-dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/self-deps
created_at: 2024-12-07T14:10:47Z
updated_at: 2024-12-09T15:42:06Z
url: https://github.com/astral-sh/uv/pull/9705
synced_at: 2026-01-10T12:00:01Z
```

# Enforce correctness of self-dependencies

---

_Pull request opened by @charliermarsh on 2024-12-07 14:10_

## Summary

As far as I can tell, this was added in https://github.com/astral-sh/uv/pull/319, but it seems _incorrect_ to ignore these.

Closes https://github.com/astral-sh/uv/issues/9693.


---

_Label `bug` added by @charliermarsh on 2024-12-07 14:10_

---

_Comment by @zanieb on 2024-12-07 15:40_

Hm I'm a little worried about the implications of this. Will it break people with dynamic versions?

---

_Comment by @charliermarsh on 2024-12-07 15:59_

Why would it break people with dynamic versions?

---

_Comment by @charliermarsh on 2024-12-07 16:03_

(This only affects packages that declare dependencies on themselves, which is itself almost always a mistake.)

---

_Comment by @zanieb on 2024-12-07 16:04_

I'm imagining you have a self-dependency with a version constraint that.. works in production but fails locally due some weird dynamic development version scheme? Perhaps I'm overthinking it. Dynamic versions stood out to me as a case where there are weird local versions, but I don't think that's the only case where there could be a mismatch.

More broadly, what happens with a self-dependency in production i.e. published to PyPI?

---

_Comment by @charliermarsh on 2024-12-07 16:07_

> More broadly, what happens with a self-dependency in production i.e. published to PyPI?

Can you expand on this? If you install the package from PyPI, and it declares a dependency on itself, they will just resolve to the same package. The same is true if you clone the package and install it locally. Both would work fine.

The case we would fail is: you declare a self-dependency, but at a version that is incompatible with the current package version. I think that should fail! Check out the linked issue where we silently succeed with a broken resolution.

(I don’t know that there’s even a real, valid use-case for a self-dependency.)

---

_Comment by @zanieb on 2024-12-07 16:10_

I did read the linked issue :) just trying to understand the problem more broadly since it turns something that's currently allowed into a firm error.

> The case we would fail is: you declare a self-dependency, but at a version that is incompatible with the current package version. I think that should fail!

I guess the clarifying question is: would this fail at publish or install time to / from PyPI? (entirely separately from uv)

---

_Comment by @charliermarsh on 2024-12-07 16:27_

By “publish time”, do you mean, if you ran uv lock locally prior to publishing?

---

_Comment by @zanieb on 2024-12-07 16:32_

I mean.. if you weren't using uv at all, i.e., some lower-level tools like `python -m build` and `twine`.

---

_Comment by @charliermarsh on 2024-12-07 16:33_

Hmm, none of those tools have to resolve dependencies though. There’s no dependency validation. They just have to construct the metadata. They don’t have to solve the graph.

---

_Comment by @zanieb on 2024-12-07 16:52_

Yeah so then when you install with `pip` later what would happen? Perhaps there would be a failure then?

---

_Comment by @charliermarsh on 2024-12-07 17:01_

Yeah it would fail then. Though I think that’s the same as if, e.g., the package declared dependencies on both flask==3.0.0 and flask==2.0.0? It would fail on install, but not on publish.

---

_Comment by @zanieb on 2024-12-07 17:03_

Okay, thanks for going through it with me. I'm not quite sure where this change could cause problems 👍 

> (I don’t know that there’s even a real, valid use-case for a self-dependency.)

I guess it'd let you declare a default extra that.. you can't turn off? 😬 

---

_Review requested from @BurntSushi by @charliermarsh on 2024-12-07 18:03_

---

_Review requested from @zanieb by @charliermarsh on 2024-12-07 18:03_

---

_Review requested from @konstin by @charliermarsh on 2024-12-07 18:03_

---

_Marked ready for review by @charliermarsh on 2024-12-07 18:03_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:19255 on 2024-12-07 18:03_

Gah, need to fix this error message.

---

_@charliermarsh reviewed on 2024-12-07 18:03_

---

_@zanieb reviewed on 2024-12-07 18:26_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:19255 on 2024-12-07 18:26_

I thought we already had a hint for self dependencies 🤔 

---

_@charliermarsh reviewed on 2024-12-07 18:27_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/snapshots/it__ecosystem__transformers-lock-file.snap`:4455 on 2024-12-07 18:27_

We could choose to omit these? This is a useless self-dependency. I retained them but we could omit them when converting from PubGrub resolution to our own graph.

---

_@charliermarsh reviewed on 2024-12-07 18:27_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:19255 on 2024-12-07 18:27_

Kind of weak.

---

_@charliermarsh reviewed on 2024-12-07 18:28_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:19765 on 2024-12-07 18:28_

Kind of weak.

---

_@zanieb reviewed on 2024-12-07 18:41_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:19255 on 2024-12-07 18:41_

I was thinking of https://github.com/astral-sh/uv/pull/7505 (which may help guide the messaging here?)

---

_@zanieb reviewed on 2024-12-07 18:43_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:19255 on 2024-12-07 18:43_

I'd include the specifier and the version maybe?

```suggestion
      ╰─▶ Because your project depends on itself (project==0.2.0) at an incompatible version (currently 0.1.0), we can conclude that your project's requirements are unsatisfiable.
```

---

_@charliermarsh reviewed on 2024-12-07 19:18_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:19255 on 2024-12-07 19:18_

I actually don't think I can get the version here right now :/

---

_@charliermarsh reviewed on 2024-12-07 19:18_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:19255 on 2024-12-07 19:18_

I added hints!

---

_@zanieb approved on 2024-12-07 21:20_

---

_Merged by @charliermarsh on 2024-12-08 04:17_

---

_Closed by @charliermarsh on 2024-12-08 04:17_

---

_Branch deleted on 2024-12-08 04:17_

---

_Comment by @notatallshaw-gts on 2024-12-09 15:40_

Just want to check this doesn't break the real use case of using extras to depend on other extras of your own project? e.g.

```toml
[project]
name = "spam-eggs"
version = "2020.0.0"
requires-python = ">=3.8"

[project.optional-dependencies]
tests = ["pytest"]
dev = ["pdb", "memray"]
all = [
  "spam-eggs[tests]",
  "spam-eggs[dev]",
]
```


---

_Comment by @charliermarsh on 2024-12-09 15:41_

No, that remains totally fine.

---

_Comment by @charliermarsh on 2024-12-09 15:41_

It _would_ error if you did:
```toml
[project]
name = "spam-eggs"
version = "2020.0.0"
requires-python = ">=3.8"

[project.optional-dependencies]
tests = ["pytest"]
all = [
  # Incompatible version!
  "spam-eggs[tests]==2021.0.0",
]
```

---
