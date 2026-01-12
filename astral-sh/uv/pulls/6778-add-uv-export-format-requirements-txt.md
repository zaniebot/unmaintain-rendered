```yaml
number: 6778
title: "Add `uv export --format requirements.txt`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/export
created_at: 2024-08-28T21:49:54Z
updated_at: 2024-08-30T17:12:47Z
url: https://github.com/astral-sh/uv/pull/6778
synced_at: 2026-01-12T16:07:31Z
```

# Add `uv export --format requirements.txt`

---

_@charliermarsh_

## Summary

The interface here is intentionally a bit more limited than `uv pip compile`, because we don't want `requirements.txt` to be a system of record -- it's just an export format. So, we don't write annotation comments (i.e., which dependency is requested from which), we don't allow writing extras, etc. It's just a flat list of requirements, with their markers and hashes.

Closes #6007.

Closes #6668.

Closes #6670.


---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2787 on 2024-08-28 22:03_

Right now this is a required argument, even though we only support one format. We could make it optional, or even remove it? But if we add formats in the future, it will then be breaking.

---

_@charliermarsh reviewed on 2024-08-28 22:03_

---

_@zanieb reviewed on 2024-08-28 22:10_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2787 on 2024-08-28 22:10_

I don't mind this, kind of funny though. What else might we support?

---

_@charliermarsh reviewed on 2024-08-28 22:27_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2787 on 2024-08-28 22:27_

Perhaps some world in which we support PEP 751 if we end up with a more narrow PEP that doesn't do universal locking?

---

_Review requested from @zanieb by @charliermarsh on 2024-08-28 22:27_

---

_Review requested from @konstin by @charliermarsh on 2024-08-28 22:27_

---

_Marked ready for review by @charliermarsh on 2024-08-28 22:27_

---

_@charliermarsh reviewed on 2024-08-28 22:38_

---

_Review comment by @charliermarsh on `crates/uv/tests/export.rs`:163 on 2024-08-28 22:38_

Wait... I thought this was a cool feature, but I don't think there's any way to "activate" these from `uv pip install` or `pip install`...? So we might need the `export` API to accept extras, to tell us which to include.

---

_@charliermarsh reviewed on 2024-08-28 22:39_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2787 on 2024-08-28 22:39_

Actually, we can just make this an optional with a default.

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:2778 on 2024-08-29 08:30_

As written (kebab-case), only `requirements-txt` (dash, not dot) is supported

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:2784 on 2024-08-29 08:31_

More nitpick: We're always in a workspace (`uv export --package dummy` works with single `pyproject.toml` `project.name = "dummy"`)

```suggestion
    /// If the workspace member does not exist, uv will exit with an error.
```


---

_Review comment by @konstin on `crates/uv/tests/export.rs`:26 on 2024-08-29 08:46_

Setuptools already requires wheel nowadays (https://setuptools.pypa.io/en/latest/build_meta.html#how-to-use-it)

```suggestion
        requires = ["setuptools>=42"]
```

---

_Review comment by @konstin on `crates/uv/tests/export.rs`:319 on 2024-08-29 08:48_

Could you add a comment on why we expect the markers to disappear? The combination of `python_version > '3.11'` and `requires_python = ">=3.12"` is hard to spot.

---

_@konstin approved on 2024-08-29 09:14_

Looks good! Only some nitpicks

---

_@charliermarsh reviewed on 2024-08-29 14:32_

---

_Review comment by @charliermarsh on `crates/uv/tests/export.rs`:26 on 2024-08-29 14:32_

Oh nice. I will change this separately since it's in all tests.

---

_@zanieb reviewed on 2024-08-29 17:14_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2817 on 2024-08-29 17:14_

Can we write a bit more here? What are the implications for a user running an export here?  e.g.

> Do not update the `uv.lock` before exporting.
>
> If a `uv.lock` does not exist, uv will exit with an error.

Did you copy these from somewhere? I thought I touched most of these up to say stuff like that.

---

_@zanieb reviewed on 2024-08-29 17:15_

---

_Review comment by @zanieb on `crates/uv/tests/help.rs`:705 on 2024-08-29 17:15_

Should we say `project's` for consistency?

---

_@zanieb approved on 2024-08-29 17:16_

---

_@charliermarsh reviewed on 2024-08-29 17:24_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2817 on 2024-08-29 17:24_

The `LockArgs` one.

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2817 on 2024-08-29 17:31_

Ah, that one is the simplest because it's for `uv lock`. I'd look at the styling in `uv sync`

```
      --locked
          Assert that the `uv.lock` will remain unchanged.
          
          Requires that the lockfile is up-to-date. If the lockfile is missing or needs to be updated, uv will exit with an error.

      --frozen
          Sync without updating the `uv.lock` file.
          
          Instead of checking if the lockfile is up-to-date, uses the versions in the lockfile as the source of truth. If the lockfile
          is missing, uv will exit with an error. If the `pyproject.toml` includes changes to dependencies that have not been included
          in the lockfile yet, they will not be present in the environment.
```

---

_@zanieb reviewed on 2024-08-29 17:31_

---

_Merged by @charliermarsh on 2024-08-29 17:46_

---

_Closed by @charliermarsh on 2024-08-29 17:46_

---

_Branch deleted on 2024-08-29 17:46_

---

_Comment by @helderco on 2024-08-30 17:12_

Pretty cool, this will help migrate to `uv.lock` because users will have a way to revert easily if needed. 🎉

---
