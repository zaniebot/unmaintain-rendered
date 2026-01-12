```yaml
number: 14413
title: "Build `path` sources without build systems by default"
type: pull_request
state: merged
author: jtfmumm
labels:
  - enhancement
  - breaking
assignees: []
merged: true
base: release/080
head: jtfm/package-default
created_at: 2025-07-02T09:28:38Z
updated_at: 2025-07-16T21:17:03Z
url: https://github.com/astral-sh/uv/pull/14413
synced_at: 2026-01-12T16:11:12Z
```

# Build `path` sources without build systems by default

---

_@jtfmumm_

We currently treat path sources as virtual if they do not specify a build system, which is surprising behavior. This PR updates the behavior to treat path sources as packages unless the path source is explicitly marked as `package = false` or its own `tool.uv.package` is set to `false`.

Closes #12015


---

_Label `enhancement` added by @jtfmumm on 2025-07-02 09:28_

---

_Label `breaking` added by @jtfmumm on 2025-07-02 09:28_

---

_@jtfmumm reviewed on 2025-07-02 09:30_

---

_Review comment by @jtfmumm on `crates/uv-workspace/src/pyproject.rs`:84 on 2025-07-02 09:30_

I left the existing logic in place in `is_package` (factoring out the `tool.uv.package` check) to constrain this change to path sources only. 

---

_Comment by @zanieb on 2025-07-02 14:20_

This should be based on #14164 right?

---

_Comment by @jtfmumm on 2025-07-02 15:11_

> This should be based on #14164 right?

Yes, updated

---

_Added to milestone `v0.8.0` by @jtfmumm on 2025-07-02 15:13_

---

_@konstin reviewed on 2025-07-02 16:23_

We need to update https://docs.astral.sh/uv/reference/settings/#package in conjunction with this change. 

---

_Comment by @jtfmumm on 2025-07-02 16:40_

> We need to update https://docs.astral.sh/uv/reference/settings/#package in conjunction with this change.

This doesn’t actually change the behavior there. It’s limited to updating the default behavior when a path source definition has no package value. 

But we can discuss if we want this to hold more generally. I was trying to limit the scope of the change to the issue. 

---

_Renamed from "No longer treat path sources as virtual if missing build specification" to "Build `path` sources without build systems by default" by @zanieb on 2025-07-02 16:45_

---

_Comment by @zanieb on 2025-07-02 16:46_

Can you add test coverage for cases where there are various `package = ..` options set? 

---

_Comment by @jtfmumm on 2025-07-02 16:54_

> Can you add test coverage for cases where there are various `package = ..` options set?

There are already existing tests but I can point them out

---

_Comment by @zanieb on 2025-07-02 18:11_

Yes please. I'm particularly interested in `package = true` / `package = false` conflicts.

---

_Comment by @jtfmumm on 2025-07-02 18:34_

* `sync_override_package` on `main` tests for a dependency with a build specification:
  * `tool.uv.package` is unset
    * override with `package = false`: does not install
    * no override: does install
  * `tool.uv.package` is `false`
    * override with `package = true`: does install
    * no override: doesn't install
  * In this PR, I added:
    * `tool.uv.package` is `true`
      * override with `package = false`: does not install
      * no override: does install

* `lock_implicit_package_path` (renamed) is updated in this PR to check that the default behavior is to build even when the path source itself had no build specification and no `tool.uv.package` value. This is testing the core behavior for this PR.

---

_Comment by @zanieb on 2025-07-02 19:41_

There should be _some_ documentation change here. Perhaps in https://docs.astral.sh/uv/concepts/projects/dependencies/#path

---

_@zanieb reviewed on 2025-07-03 14:25_

---

_Review comment by @zanieb on `docs/concepts/projects/dependencies.md`:441 on 2025-07-03 14:25_

Which "project" are you referring to here?

---

_@zanieb reviewed on 2025-07-03 14:27_

---

_Review comment by @zanieb on `docs/concepts/projects/dependencies.md`:441 on 2025-07-03 14:27_

Can you wrap the line so it's ~close to the line length in the rest of the file? The autoformatter can't wrap admonitions.



---

_@zanieb reviewed on 2025-07-03 14:27_

---

_Review comment by @zanieb on `docs/concepts/projects/dependencies.md`:442 on 2025-07-03 14:27_

I think this link will break if split across lines.

---

_@zanieb reviewed on 2025-07-03 14:28_

---

_Review comment by @zanieb on `docs/concepts/projects/dependencies.md`:442 on 2025-07-03 14:28_

Why "build specification"? Is that a term we use elsewhere? We should just say "build system" for consistency?

---

_@zanieb reviewed on 2025-07-03 14:28_

---

_Review comment by @zanieb on `docs/concepts/projects/dependencies.md`:441 on 2025-07-03 14:28_

It feels weird this is first framed around "If the project is marked as a non-package", since that's the advanced use-case. The more common scenario is that the dependency just doesn't declare a build system.

---

_@zanieb reviewed on 2025-07-03 14:29_

---

_Review comment by @zanieb on `docs/concepts/projects/dependencies.md`:439 on 2025-07-03 14:29_

We should probably change this from an admonition to a whole section, it's getting long and then we can link to it.

---

_Review comment by @jtfmumm on `docs/concepts/projects/dependencies.md`:441 on 2025-07-03 14:34_

Here I was trying to emphasize the combination of `package` values since the point about building dependencies without a declared build system is already covered in the build systems section of the docs.

---

_@jtfmumm reviewed on 2025-07-03 14:34_

---

_@zanieb reviewed on 2025-07-03 14:56_

---

_Review comment by @zanieb on `docs/concepts/projects/dependencies.md`:441 on 2025-07-03 14:56_

That section is about building the current project, where-as here we need to talk about how we build projects when they're dependencies.

---

_Review comment by @konstin on `docs/concepts/projects/dependencies.md`:415 on 2025-07-04 09:51_

```suggestion
[`[build-system]` table](./config.md#build-systems). If you'd like to override this behavior and
```

---

_@konstin reviewed on 2025-07-04 09:52_

---

_@T-256 reviewed on 2025-07-04 21:31_

---

_Review comment by @T-256 on `docs/concepts/projects/dependencies.md`:420 on 2025-07-04 21:31_

Invalid `project` table? (missing name and version)

---

_Review comment by @jtfmumm on `docs/concepts/projects/dependencies.md`:420 on 2025-07-07 07:58_

This is following a pattern found throughout this file. If we want to change that, I think it's out of scope here.

---

_@jtfmumm reviewed on 2025-07-07 07:58_

---

_Comment by @zanieb on 2025-07-15 17:55_

I posted some documentation edits at https://github.com/astral-sh/uv/pull/14632

I've rolled it out of the "Path" section because I think it was getting too long and nested.

---

_@zanieb approved on 2025-07-16 21:16_

---

_Merged by @zanieb on 2025-07-16 21:17_

---

_Closed by @zanieb on 2025-07-16 21:17_

---

_Branch deleted on 2025-07-16 21:17_

---
