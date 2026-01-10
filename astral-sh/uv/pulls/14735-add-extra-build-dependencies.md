```yaml
number: 14735
title: "Add `extra-build-dependencies`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: zb/extra-build-dependencies
created_at: 2025-07-18T17:37:54Z
updated_at: 2025-07-30T14:53:09Z
url: https://github.com/astral-sh/uv/pull/14735
synced_at: 2026-01-10T06:53:02Z
```

# Add `extra-build-dependencies`

---

_Pull request opened by @zanieb on 2025-07-18 17:37_

Replaces https://github.com/astral-sh/uv/pull/14092

Adds `tool.uv.extra-build-dependencies = {package = [dependency, ...]}` which extends `build-system.requires` during package builds.

These are lowered via workspace sources, are applied to transitive dependencies, and are included in the wheel cache shard hash.

There are some features we need to follow-up on, but are out of scope here:

- Preferring locked versions for build dependencies
- Settings for requiring locked versions for build depencies

There are some quality of life follow-ups we should also do:

- Warn on `extra-build-dependencies` that do not apply to any packages
- Add test cases and improve error messaging when the `extra-build-dependencies` resolve fails


-------

There ~are~ were a few open decisions to be made here

1. Should we resolve these dependencies alongside the `build-system.requires` dependencies? Or should we resolve separately? (I think the latter is more powerful? because you can override things? but it opens the door to breaking your build)
2. Should we install these dependencies into the same environment? Or should we layer it on top as we do elsewhere? (I think it's fine to install into the same environment)
3. Should we respect sources defined in the parent project? (I think yes, but then we need to lower the dependencies earlier — I don't think that's a big deal, but it's not implemented)
4. Should we respect sources defined in the child project? (I think no, this gets really complicated and seems weird to allow)
5. Should we apply this to transitive dependencies? (I think so)


---

_Label `enhancement` added by @zanieb on 2025-07-18 17:37_

---

_Label `preview` added by @zanieb on 2025-07-18 17:37_

---

_@zanieb reviewed on 2025-07-18 17:45_

---

_Review comment by @zanieb on `crates/uv-build-frontend/src/lib.rs`:559 on 2025-07-18 17:45_

There's some unreverted noise in this file from Aria's change. I'll clean it up.

---

_@zanieb reviewed on 2025-07-18 17:46_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:1574 on 2025-07-18 17:46_

Thank you <3 @konstin for the deranged test case

---

_Comment by @charliermarsh on 2025-07-20 17:23_

> Should we resolve these dependencies alongside the build-system.requires dependencies? Or should we resolve separately? (I think the latter is more powerful? because you can override things? but it opens the door to breaking your build)

I would probably resolve them together. At least until we see a use-case to resolve them separately. (We could add build dependency overrides to allow explicit overrides, like we have build dependency constraints.)

> Should we install these dependencies into the same environment? Or should we layer it on top as we do elsewhere. (I think it's fine to install into the same environment)

Same environment IMO.

> Should we respect sources defined in the parent project? (I think yes, but then we need to lower the dependencies earlier)

I think yes... I think we should respect the sources defined in the same file as the `extra-build-dependencies` definition, for consistency.

> Should we respect sources defined in the child project? (I think no)

I don't think so.

> Should we apply this to transitive dependencies? (I think so)

I think so, yeah. Similar to other settings, like `--config-settings-package`.


---

_Marked ready for review by @zanieb on 2025-07-28 16:41_

---

_Review requested from @konstin by @zanieb on 2025-07-29 17:14_

---

_Review requested from @Gankra by @zanieb on 2025-07-29 17:14_

---

_Review comment by @charliermarsh on `crates/uv-bench/benches/uv.rs`:144 on 2025-07-30 01:18_

I think you should implement `Default` on `uv_distribution::ExtraBuildRequires`, so this can just be `ExtraBuildRequires::default`.

---

_@charliermarsh reviewed on 2025-07-30 01:18_

---

_Review comment by @charliermarsh on `crates/uv-dispatch/src/lib.rs`:226 on 2025-07-30 01:19_

Sort of prefer using just `ExtraBuildDependencies` here rather than the fully-qualified path, for consistency.

---

_@charliermarsh reviewed on 2025-07-30 01:19_

---

_@charliermarsh reviewed on 2025-07-30 01:20_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/build_requires.rs`:291 on 2025-07-30 01:20_

Should this just be a `From<ExtraBuildDependencies>` impl?

---

_@charliermarsh reviewed on 2025-07-30 01:21_

---

_Review comment by @charliermarsh on `crates/uv-pep508/src/lib.rs`:278 on 2025-07-30 01:21_

It could make sense to impl it for `Version` if you feel motivated, to avoid this allocation.

---

_@charliermarsh reviewed on 2025-07-30 01:21_

---

_Review comment by @charliermarsh on `crates/uv-pep508/src/lib.rs`:283 on 2025-07-30 01:21_

Is the `to_string` because it's generic? I wonder if we could require `Pep508Url` to impl `CacheKey`?

---

_Review comment by @charliermarsh on `crates/uv-settings/src/settings.rs`:635 on 2025-07-30 01:22_

Extra comma after "like" at the end.

---

_@charliermarsh reviewed on 2025-07-30 01:22_

---

_@charliermarsh reviewed on 2025-07-30 01:23_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:2180 on 2025-07-30 01:23_

I generally prefer `ExtraBuildRequires` here rather than the qualified path, unless this is more consistent with the rest of the file and the diff obscures it.

---

_@charliermarsh reviewed on 2025-07-30 01:24_

---

_Review comment by @charliermarsh on `scripts/packages/anyio_local/anyio/__init__.py`:2 on 2025-07-30 01:24_

(Missing newline, doesn't really matter.)

---

_@charliermarsh approved on 2025-07-30 01:24_

Nice work.

---

_Review comment by @konstin on `docs/reference/settings.md`:2699 on 2025-07-30 08:57_

This example is wrong

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject.rs`:783 on 2025-07-30 09:08_

dead code

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject.rs`:773 on 2025-07-30 09:09_

What about merging those two into a single newtype `ExtraBuildDependencies` with a `Deref` on it?

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject.rs`:790 on 2025-07-30 09:13_

We're copy and pasting this snipped for each new map, can we put it in a utility (https://github.com/serde-rs/serde/issues/1607#issuecomment-525577524)?

---

_Review comment by @konstin on `crates/uv-build-frontend/src/lib.rs`:367 on 2025-07-30 09:28_

Can you add an `if !extra_build_dependencies.is_empty()` "requires and extra-build-requires"? Or change it to something that doesn't mention the field name at all.

---

_Review comment by @konstin on `crates/uv-build-frontend/src/lib.rs`:508 on 2025-07-30 09:31_

I think that's always correct to solve in a single, so we get a consistent requirement. We could add an option to ignore the default build deps if we actually need.

---

_Review comment by @konstin on `crates/uv-distribution/src/metadata/build_requires.rs`:291 on 2025-07-30 09:34_

This separate method avoids accidentally converting them without lowering

---

_@konstin approved on 2025-07-30 10:13_

---

_@zanieb reviewed on 2025-07-30 11:42_

---

_Review comment by @zanieb on `crates/uv-distribution/src/metadata/build_requires.rs`:291 on 2025-07-30 11:42_

Yeah I think a dedicated method makes sense here.

---

_@zanieb reviewed on 2025-07-30 11:44_

---

_Review comment by @zanieb on `crates/uv-pep508/src/lib.rs`:283 on 2025-07-30 11:44_

Yeah I think so, I looked at it briefly and it the generic aspect seemed tricky.

---

_Merged by @zanieb on 2025-07-30 14:53_

---

_Closed by @zanieb on 2025-07-30 14:53_

---

_Branch deleted on 2025-07-30 14:53_

---
