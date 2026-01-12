```yaml
number: 13317
title: "make `uv version` lock and sync"
type: pull_request
state: merged
author: Gankra
labels:
  - bug
assignees: []
merged: true
base: main
head: gankra/sync-version
created_at: 2025-05-06T18:54:50Z
updated_at: 2025-05-21T13:46:11Z
url: https://github.com/astral-sh/uv/pull/13317
synced_at: 2026-01-12T16:10:39Z
```

# make `uv version` lock and sync

---

_@Gankra_

This adopts the logic from `uv remove` for locking and syncing, as the scope of the changes made are ultimately similar. Unlike `uv remove` there is no support for modifying PEP723 scripts, as these are not versioned.

In doing this the `version` command gains a truckload of args for configuring lock/sync behaviour. Presumably most of these are passed via settings or env files, and not of particular concern. 

The most interesting additions are:

* `--frozen`: makes `uv version` work ~exactly as it did before this PR
* `--locked`: errors if the lockfile is out of date
* `--no-sync`: updates the lockfile, but doesn't run the equivalent of `uv sync`
* `--package name`: a convenience for referring to a package in the workspace

Note that the existing `--dry-run` flag effectively implies `--frozen`.

Fixes #13254
Fixes #13548

---

_Comment by @Gankra on 2025-05-06 18:58_

Note for reviewers: `print_version`, `bumped_version`, and similar "compute the version change" logic should be completely unchanged. All the project read/write logic was completely rewritten and can be reviewed in a vacuum. 

The added code is nearly an exact copy paste from `uv::commands::project::remove::remove`, but with the support for `RemoveTarget::Script` removed.

---

_Label `bug` added by @Gankra on 2025-05-06 19:03_

---

_Comment by @Gankra on 2025-05-06 19:03_

I should almost certainly add some new tests for this...

---

_Comment by @Gankra on 2025-05-06 19:14_

Some new sample outputs

```
$ uv version
myfast 1.2.8
```

```
$ uv version 1.2.9
Resolved 36 packages in 7ms
Audited 34 packages in 0.02ms
myfast 1.2.8 => 1.2.9
```

```
$ uv version 1.2.10 --no-sync
Resolved 36 packages in 7ms
myfast 1.2.9 => 1.2.10
```

```
$ uv version 1.2.11 --frozen 
myfast 1.2.10 => 1.2.11
```

```
uvloc version 1.2.12 --locked 
Resolved 36 packages in 7ms
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
```

...it occurs to me that `--locked` is basically an unusable cli flag for add/remove/version but well, that's a pre-existing truth I guess. I suppose it works for:

* `uv version --locked` (no values to set)
* `uv version --locked the-version-thats-already-there` (noop)

---

_Comment by @konstin on 2025-05-06 22:54_

Can you add a test for the pathological but motivating case where a registry package depends on the bumped package, causing a conflict if not updated? So that we cover why we need the full set of resolver-installer options.

---

_@charliermarsh approved on 2025-05-08 00:08_

This looks great! The one thing that's missing is a few tests to verify that the lockfile is _actually_ getting updated.

---

_Comment by @HenriBlacksmith on 2025-05-20 07:43_

Any plans to merge this fix?

---

_@Gankra reviewed on 2025-05-20 17:57_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/version.rs`:337 on 2025-05-20 17:57_

This logic for mapping the package to the version is extremely dubious to me...

---

_@Gankra reviewed on 2025-05-20 17:58_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/version.rs`:109 on 2025-05-20 17:58_

this is how we get the "name" we'll be looking up in the lockfile

---

_@Gankra reviewed on 2025-05-20 17:59_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/version.rs`:240 on 2025-05-20 17:59_

This is how we find the project we use to lookup the pyproject.toml to get the name from

---

_@Gankra reviewed on 2025-05-20 18:06_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/version.rs`:109 on 2025-05-20 18:06_

I think this part can at least be simplified to `project.project_name()`

---

_@zanieb reviewed on 2025-05-20 18:44_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/version.rs`:213 on 2025-05-20 18:44_

Nit: This is the proper style
```suggestion
/// Note that `uv version` never needs to support PEP 723 scripts, as those are unversioned.
```

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/version.rs`:105 on 2025-05-20 19:26_

I think if we don't have a `name`, then we don't have a `[project]` table, so we don't have a version?

---

_@charliermarsh reviewed on 2025-05-20 19:26_

---

_@charliermarsh reviewed on 2025-05-21 13:02_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/version.rs`:102 on 2025-05-21 13:02_

I think I'd stylize this as: "Missing `project.name` field in: {}" (note the backticks)

---

_Merged by @Gankra on 2025-05-21 13:46_

---

_Closed by @Gankra on 2025-05-21 13:46_

---

_Branch deleted on 2025-05-21 13:46_

---
