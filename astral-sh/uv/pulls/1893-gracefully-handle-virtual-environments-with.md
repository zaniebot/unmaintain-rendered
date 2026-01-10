```yaml
number: 1893
title: Gracefully handle virtual environments with conflicting packages
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dupes
created_at: 2024-02-23T02:15:17Z
updated_at: 2024-02-23T19:14:53Z
url: https://github.com/astral-sh/uv/pull/1893
synced_at: 2026-01-10T14:54:43Z
```

# Gracefully handle virtual environments with conflicting packages

---

_Pull request opened by @charliermarsh on 2024-02-23 02:15_

## Summary

When a virtual environment contains multiple packages with the same name, we no longer throw a hard error. Instead:

- In `uv pip freeze`, we list all versions.
- In `uv pip uninstall`, we uninstall all versions.
- In `uv pip install`, we uninstall all versions prior to installing a new version.

Closes https://github.com/astral-sh/uv/issues/1848.


---

_Label `bug` added by @charliermarsh on 2024-02-23 02:15_

---

_Review requested from @konstin by @charliermarsh on 2024-02-23 02:38_

---

_Review requested from @zanieb by @charliermarsh on 2024-02-23 02:39_

---

_Review comment by @konstin on `crates/uv-installer/src/site_packages.rs`:104 on 2024-02-23 11:23_

```suggestion
    /// Returns the versions of the given package, if it is installed.
```

---

_Review comment by @konstin on `crates/uv-installer/src/site_packages.rs`:115 on 2024-02-23 11:24_

```suggestion
    /// Remove the given packages from the index, returning all installed versions, if any.
```

---

_Review comment by @konstin on `crates/uv/tests/common/mod.rs`:372 on 2024-02-23 11:28_

```suggestion
    fs_err::create_dir_all(&dst)?;
```

---

_Review comment by @konstin on `crates/uv/tests/common/mod.rs`:379 on 2024-02-23 11:28_

```suggestion
            fs_err::copy(entry.path(), dst.as_ref().join(entry.file_name()))?;
```

---

_@konstin approved on 2024-02-23 11:30_

I'd leave some comments about why the versions are a vec instead of a single installed version

---

_@MichaReiser reviewed on 2024-02-23 11:45_

---

_Review comment by @MichaReiser on `crates/uv/tests/common/mod.rs`:379 on 2024-02-23 11:45_

Is this true in all cases? Should we set up https://rust-lang.github.io/rust-clippy/master/index.html#/disallowed_methods

---

_@konstin reviewed on 2024-02-23 12:05_

---

_Review comment by @konstin on `crates/uv/tests/common/mod.rs`:379 on 2024-02-23 12:05_

There are some cases where we need `std::fs` (e.g. there's no equivalent to `std::fs::Permissions`), otherwise i'd always use `fs_err`, it helps tremendously with debugging.

---

_@MichaReiser reviewed on 2024-02-23 12:27_

---

_Review comment by @MichaReiser on `crates/uv/tests/common/mod.rs`:379 on 2024-02-23 12:27_

It sounds like we could use `disallowed_methods` for the methods that are e.g. not `fs::Permission`

---

_@zanieb reviewed on 2024-02-23 15:16_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:379 on 2024-02-23 15:16_

That'd be nice to have

---

_@zanieb reviewed on 2024-02-23 15:17_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:379 on 2024-02-23 15:17_

https://github.com/astral-sh/uv/issues/1916

---

_Merged by @charliermarsh on 2024-02-23 19:14_

---

_Closed by @charliermarsh on 2024-02-23 19:14_

---

_Branch deleted on 2024-02-23 19:14_

---
