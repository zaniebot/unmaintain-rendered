```yaml
number: 956
title: Add support for disabling installation from pre-built wheels
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/no-binary
created_at: 2024-01-17T23:09:09Z
updated_at: 2024-01-19T17:24:28Z
url: https://github.com/astral-sh/uv/pull/956
synced_at: 2026-01-12T16:04:19Z
```

# Add support for disabling installation from pre-built wheels

---

_@zanieb_

Adds support for disabling installation from pre-built wheels i.e. the package must be built from source locally. 
We will still always use pre-built wheels for metadata during resolution.

Available via `--no-binary` and `--no-binary-package <name>` flags in `pip install` and `pip sync`. There is no flag for `pip compile` since no installation happens there.

```
--no-binary

    Don't install pre-built wheels.
    
    When enabled, all installed packages will be installed from a source distribution. 
    The resolver will still use pre-built wheels for metadata.


--no-binary-package <NO_BINARY_PACKAGE>

    Don't install pre-built wheels for a specific package.
    
    When enabled, the specified packages will be installed from a source distribution. 
    The resolver will still use pre-built wheels for metadata.
```

When packages are already installed, the `--no-binary` flag will have no affect without the `--reinstall` flag. In the future, I'd like to change this by tracking if a local distribution is from a pre-built wheel or a locally-built wheel. However, this is significantly more complex and different than `pip`'s behavior so deferring for now.

For reference, `pip`'s flag works as follows:

```
--no-binary <format_control>

    Do not use binary packages. Can be supplied multiple times, and each time adds to the
    existing value. Accepts either ":all:" to disable all binary packages, ":none:" to empty the
    set (notice the colons), or one or more package names with commas between them (no colons).
    Note that some packages are tricky to compile and may fail to install when this option is
    used on them.
```

Note we are not matching the exact `pip` interface here because it seems complicated to use. I think we may want to consider adjusting our interface for this behavior since we're not entirely compatible anyway e.g. I think `--force-build` and `--force-build-package` are clearer names. We could also consider matching the `pip` interface or only allowing `--no-binary <package>` for compatibility. We can of course do whatever we want in our _own_ install interfaces later.

Additionally, we may want to further consider the semantics of `--no-binary`. For example, if I run `pip install pydantic --no-binary` I expect _just_ Pydantic to be installed without binaries but by default we will build all of Pydantic's dependencies too.

This work was prompted by #895, as it is much easier to measure performance gains from building source distributions if we have a flag to ensure we actually build source distributions. Additionally, this is a flag I have used frequently in production to debug packages that ship Cythonized wheels.


---

_Renamed from "Add `--no-binary` and `--no-binary-package <name>`" to "Add support for disabling installation from pre-built wheels" by @zanieb on 2024-01-17 23:10_

---

_Label `enhancement` added by @zanieb on 2024-01-17 23:19_

---

_@zanieb reviewed on 2024-01-17 23:28_

---

_Review comment by @zanieb on `crates/puffin-traits/src/lib.rs`:164 on 2024-01-17 23:28_

Please do correct me if there's somewhere better for this.

I tried to put it alongside `Reinstall` but it's needed in the resolver as well so I put it in this shared crate.

It's [still re-exported in the installer](https://github.com/astral-sh/puffin/pull/956/files#diff-f207c79e0088b89d8661e116139d2c6b61a3221e3035993341818bb391d2cbdeR5-R6).

---

_@zanieb reviewed on 2024-01-17 23:31_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install.rs`:749 on 2024-01-17 23:31_

These tests are slow (~5s). Maybe I should use a scenario that builds faster?

---

_@zanieb reviewed on 2024-01-17 23:31_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install.rs`:749 on 2024-01-17 23:31_

It'd also be ideal to have coverage for e.g. installing a wheel via direct URL with `--no-binary` (which should fail)

---

_@zanieb reviewed on 2024-01-17 23:36_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/finder.rs`:163 on 2024-01-17 23:36_

The following made conditional and indented without change.

---

_Review requested from @charliermarsh by @zanieb on 2024-01-18 14:43_

---

_Comment by @charliermarsh on 2024-01-18 14:47_

Will review today!

---

_@charliermarsh reviewed on 2024-01-18 21:35_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/lib.rs`:6 on 2024-01-18 21:35_

Is this stale / can it be removed? It looks like you are importing from `puffin_traits` in the various files.

---

_@charliermarsh reviewed on 2024-01-18 21:36_

---

_Review comment by @charliermarsh on `crates/puffin-traits/src/lib.rs`:164 on 2024-01-18 21:36_

Seems okay -- we don't have a much better / obvious answer.

---

_@charliermarsh reviewed on 2024-01-18 21:37_

This looks good to me, nice work. My only concern is that we now have `--no-binary` and `--no-build` which does the opposite. Should those somehow be a single flag...? Should they be marked as conflicting in some sense, since providing both excludes all distributions?

---

_@zanieb reviewed on 2024-01-18 22:22_

---

_Review comment by @zanieb on `crates/puffin-installer/src/lib.rs`:6 on 2024-01-18 22:22_

Yeah commentary at https://github.com/astral-sh/puffin/pull/956#discussion_r1456615647

We should probably remove it and fix the imports that use this crate.

---

_Comment by @zanieb on 2024-01-18 22:25_

> My only concern is that we now have --no-binary and --no-build which does the opposite. Should those somehow be a single flag...? 

Well, `pip` has `--no-binary <package,:all:,:none:>` and `--only-binary <package,:all:,:none:>`. Perhaps we should change ours to match this interface?

I don't think we should explore some sort of "unified" flag in the pip-compatible interface. I think we can consider it in our own interface later.

> Should they be marked as conflicting in some sense, since providing both excludes all distributions?

Yes I think we should mark them as conflicting.

---

_Comment by @zanieb on 2024-01-18 23:03_

Opening https://github.com/astral-sh/puffin/issues/977 for the first point.

---

_@charliermarsh approved on 2024-01-19 03:23_

---

_Merged by @zanieb on 2024-01-19 17:24_

---

_Closed by @zanieb on 2024-01-19 17:24_

---

_Branch deleted on 2024-01-19 17:24_

---
