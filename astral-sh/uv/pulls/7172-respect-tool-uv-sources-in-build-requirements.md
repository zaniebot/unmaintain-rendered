```yaml
number: 7172
title: "Respect `[tool.uv.sources]` in build requirements"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/build
created_at: 2024-09-07T17:25:39Z
updated_at: 2024-10-15T15:31:05Z
url: https://github.com/astral-sh/uv/pull/7172
synced_at: 2026-01-10T12:53:41Z
```

# Respect `[tool.uv.sources]` in build requirements

---

_Pull request opened by @charliermarsh on 2024-09-07 17:25_

## Summary

We weren't respecting `tool.uv.sources` for `build-requires`.

Closes https://github.com/astral-sh/uv/issues/7147.


---

_Label `enhancement` added by @charliermarsh on 2024-09-07 17:25_

---

_Review comment by @zanieb on `crates/uv/tests/sync.rs`:2157 on 2024-09-07 17:27_

Nit: Why a workspace member instead of a local path? This seems a bit more complicated and I think we'd want to duplicate the test if we stop requiring a source definition here. Maybe worth adding both now?

---

_@zanieb reviewed on 2024-09-07 17:27_

---

_@charliermarsh reviewed on 2024-09-07 17:28_

---

_Review comment by @charliermarsh on `crates/uv/tests/sync.rs`:2157 on 2024-09-07 17:28_

Good idea, we can test both.

---

_Comment by @charliermarsh on 2024-09-07 17:34_

Ugh, the lower bound warnings are super annoying here. I guess I have to make that configurable?

---

_Review requested from @zanieb by @charliermarsh on 2024-09-07 17:53_

---

_Review requested from @konstin by @charliermarsh on 2024-09-10 02:53_

---

_Comment by @konstin on 2024-09-10 13:54_

The `[build-system]` table was created so that projects can be built by any PEP 517 build frontend. This means that you could mix and match tools, allowing people to migrate away from setuptools and for alternative to pip to being built. By reading `tool.uv.sources`, we support a uv-only feature, something that e.g. `python -m build` doesn't support. To make it clear that this doesn't build a vendor lock-in, we need to make it clear in the docs:

`tool.uv.sources` for the `build-system` table is for development only. It is our equivalent to using `pip install -e ./my-setuptools-ext` and then building without build isolation: It's our way of feature parity with pip without dropping you out if the uv project interface. We do only support it when building from a directory, we do not support it when building when from an index or a file sdist. Even `uv build --wheel --sdist` will not recognize it, but give you the default standards-only PEP 517 build experience. As a package author, you need to test that your (published) project does also builds without `tool.uv.sources` using only published packages.

---

_Comment by @mmerickel on 2024-09-10 14:31_

At some point you may want to support a find-links feature to side load a wheelhouse. You can see an example of how that works in the additional info of #7147. That would impact the lookup at all times and it does work to allow a build backend to find/resolve dependencies that are not distributed externally. As a naive user that’s basically how I think of tool.uv.sources as well. 

Separately from that when you say that `uv build --wheel` won’t use the sources that doesn’t feel like it will be ok in the long term. Build dependencies do not need to be distributed if I’m building a wheel. Only a sdist where building happens by an external system. uv should be able to build my wheel using workspace dependencies and no one else needs them. The wheel can be installed without them. 

---

_Review comment by @konstin on `crates/uv/tests/sync.rs`:2115 on 2024-09-10 15:04_

Maybe call it `my-build` or something so it's clear that this isn't about `python -m build`, otherwise `build-system.requires = ["setuptools>=42", "build"]` is easy to confuse.

---

_@konstin approved on 2024-09-10 15:07_

---

_Comment by @mmerickel on 2024-09-10 22:46_

> Even uv build --wheel --sdist will not recognize it

I guess just to poke at this more if we go back to my issue that inspired this work #7147, I'm concerned about @konstin's assertion that `uv build --wheel --package my-app` wouldn't work with this new feature? Is that true that it wouldn't use the workspace dependency to build the wheel for package my-app which depends on the local workspace package `my-setuptools-extension`? So I could `uv sync` properly but not `uv build`?

---

_Comment by @mmerickel on 2024-10-04 19:39_

Is there anything I can do to help move this forward?

---

_Comment by @charliermarsh on 2024-10-15 00:08_

I think `uv build` _would_ respect this, and you'd need to use `uv build --no-sources` to avoid it. Are you okay with that, @konstin? I think it would be surprising if `uv build` didn't run the same build process as our other hooks.

---

_Comment by @konstin on 2024-10-15 07:26_

Yeah that's what I'd suggest.

---

_Comment by @charliermarsh on 2024-10-15 12:43_

Ok, will rebase and merge this today.

---

_Merged by @charliermarsh on 2024-10-15 15:31_

---

_Closed by @charliermarsh on 2024-10-15 15:31_

---

_Branch deleted on 2024-10-15 15:31_

---
