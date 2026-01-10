```yaml
number: 7442
title: Allow users to provide pre-defined metadata for resolution
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/predefined-deps
created_at: 2024-09-16T21:14:09Z
updated_at: 2024-09-18T03:18:06Z
url: https://github.com/astral-sh/uv/pull/7442
synced_at: 2026-01-10T12:53:47Z
```

# Allow users to provide pre-defined metadata for resolution

---

_Pull request opened by @charliermarsh on 2024-09-16 21:14_

## Summary

This PR enables users to provide pre-defined static metadata for dependencies. It's intended for situations in which the user depends on a package that does _not_ declare static metadata (e.g., a `setup.py`-only sdist), and that is expensive to build or even cannot be built on some architectures. For example, you might have a Linux-only dependency that can't be built on ARM -- but we need to build that package in order to generate the lockfile. By providing static metadata, the user can instruct uv to avoid building that package at all.

For example, to override all `anyio` versions:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["anyio"]

[[tool.uv.dependency-metadata]]
name = "anyio"
requires-dist = ["iniconfig"]
```

Or, to override a specific version:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["anyio"]

[[tool.uv.dependency-metadata]]
name = "anyio"
version = "3.7.0"
requires-dist = ["iniconfig"]
```

The current implementation uses `Metadata23` directly, so we adhere to the exact schema expected internally and defined by the standards. Any entries are treated similarly to overrides, in that we won't even look for `anyio@3.7.0` metadata in the above example. (In a way, this also enables #4422, since you could remove a dependency for a specific package, though it's probably too unwieldy to use in practice, since you'd need to redefine the _rest_ of the metadata, and do that for every package that requires the package you want to omit.)

This is under-documented, since I want to get feedback on the core ideas and names involved.

Closes https://github.com/astral-sh/uv/issues/7393.


---

_Label `enhancement` added by @charliermarsh on 2024-09-16 21:14_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-09-16 21:14_

---

_Review requested from @zanieb by @charliermarsh on 2024-09-16 21:14_

---

_Review requested from @konstin by @charliermarsh on 2024-09-16 21:14_

---

_Comment by @charliermarsh on 2024-09-16 21:20_

It would be nice if we had some sort of `uv show `here to make it easy for users to add these... `uv show anyio==3.7.0`, then copy-paste into `pyproject.toml`.

---

_Comment by @charliermarsh on 2024-09-16 21:25_

I'm not a fan of the name `[[tool.uv.static-metadata]]`. I want `[[tool.uv.metadata]]` but I know that's very dicey to use here.

---

_Comment by @zanieb on 2024-09-16 21:39_

What do you think of `metadata-overrides`? Could `version` be optional for a blanket override?

(Very cool feature!)

---

_Comment by @charliermarsh on 2024-09-16 23:02_

Yes and yes.

---

_Comment by @charliermarsh on 2024-09-16 23:26_

Done and done. Should I go ahead and add docs here @zanieb? Any other concerns?

---

_Comment by @zanieb on 2024-09-16 23:49_

I don't have any other concerns!

Do we want to introduce new things like this in preview? Or just go for it? (I am leaning towards not using preview until later in uv's life)

---

_Comment by @charliermarsh on 2024-09-17 01:23_

I was thinking we'd just go for it hah.

---

_Comment by @charliermarsh on 2024-09-17 02:58_

Added docs.

---

_@charliermarsh reviewed on 2024-09-17 02:58_

---

_Review comment by @charliermarsh on `docs/concepts/resolution.md`:268 on 2024-09-17 02:58_

I would appreciate a read here.

---

_@charliermarsh reviewed on 2024-09-17 02:59_

---

_Review comment by @charliermarsh on `docs/concepts/resolution.md`:270 on 2024-09-17 02:59_

My only complaint with the name ("Metadata overrides") is that the primary goal isn't to override... It's to allow the user to provide static metadata so that we can skip builds. I think override implies changing the metadata in some way, but the goal here is just to enable users to pre-provide the true metadata.

---

_@zanieb reviewed on 2024-09-17 03:21_

---

_Review comment by @zanieb on `docs/concepts/resolution.md`:270 on 2024-09-17 03:21_

>  I think override implies changing the metadata in some way, but the goal here is just to enable users to pre-provide the true metadata.

Fair, but I think if we want to frame it that way we need to provide a way to (1) populate the metadata automatically and/or (2) validate that it matches the distribution at install time.

I wouldn't be upset about "Dependency metadata" instead if we plan to add features like (1) and (2). What do you think?

---

_@konstin approved on 2024-09-17 09:18_

---

_Review comment by @charliermarsh on `docs/concepts/resolution.md`:270 on 2024-09-17 12:32_

I do want to do (1) and (2), but I probably won't do them ASAP. I don't know what the right API looks like for (1) 🤔 

---

_@charliermarsh reviewed on 2024-09-17 12:32_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock/mod.rs`:809 on 2024-09-17 13:42_

Is this just to avoid a `use serde::Serialize;`?

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:890 on 2024-09-17 13:46_

What motivated this?

---

_Review comment by @BurntSushi on `docs/concepts/projects.md`:749 on 2024-09-17 13:49_

I think the question that comes to mind for me when reading this is, "why would I want to skip building a package?" The answer that comes immediately to mind is "for performance," but I'm not sure that's right. I think this is most useful when building a package in some circumstances is difficult or impossible, and this provides an escape hatch. Maybe that context can be added here? Basically, say more about _why_ users might want to use this feature.

---

_@BurntSushi approved on 2024-09-17 13:49_

This is quite the hammer. I like it.

---

_@charliermarsh reviewed on 2024-09-17 13:50_

---

_Review comment by @charliermarsh on `crates/uv/src/lib.rs`:890 on 2024-09-17 13:50_

I got a Clippy warning that the future was too large, I'm not sure why it appeared here, maybe because the size of the settings struct increased. Is `Box::pin` bad?

---

_@charliermarsh reviewed on 2024-09-17 13:57_

---

_Review comment by @charliermarsh on `docs/concepts/resolution.md`:270 on 2024-09-17 13:57_

I'm now leaning towards "dependency metadata".

---

_@BurntSushi reviewed on 2024-09-17 16:04_

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:890 on 2024-09-17 16:04_

No, not at all. That Clippy warning seems worth heeding without evidence to the contrary. Just curious.

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:890 on 2024-09-17 16:14_

I've made a similar change a few times.

---

_@zanieb reviewed on 2024-09-17 16:14_

---

_Comment by @charliermarsh on 2024-09-17 17:54_

I rebranded as `dependency-metadata`. From my perspective it's good to go.

---

_@zanieb reviewed on 2024-09-17 18:58_

---

_Review comment by @zanieb on `docs/concepts/projects.md`:756 on 2024-09-17 18:58_

Where does this come from? Can we link to the PyPI inspector or something?

---

_@zanieb approved on 2024-09-17 19:00_

---

_Merged by @charliermarsh on 2024-09-18 03:18_

---

_Closed by @charliermarsh on 2024-09-18 03:18_

---

_Branch deleted on 2024-09-18 03:18_

---
