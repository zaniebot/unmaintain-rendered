```yaml
number: 842
title: "Add support for `prepare_metadata_for_build_wheel`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - performance
assignees: []
merged: true
base: main
head: charlie/prepare-metadata
created_at: 2024-01-08T22:14:21Z
updated_at: 2024-01-10T00:07:38Z
url: https://github.com/astral-sh/uv/pull/842
synced_at: 2026-01-12T16:04:14Z
```

# Add support for `prepare_metadata_for_build_wheel`

---

_@charliermarsh_

## Summary

This PR adds support for `prepare_metadata_for_build_wheel`, which allows us to determine source distribution metadata without building the source distribution. This represents an optimization for the resolver, as we can skip the expensive build phase for build backends that support it.

For reference, `prepare_metadata_for_build_wheel` seems to be supported by:

- `hatchling` (as of [1.0.9](https://hatch.pypa.io/latest/history/hatchling/#hatchling-v1.9.0)).
- `flit`
- `setuptools`

In fact, it seems to work for every backend _except_ those using legacy `setup.py`.

Closes #599.


---

_@charliermarsh reviewed on 2024-01-08 22:15_

---

_Review comment by @charliermarsh on `crates/puffin-dispatch/src/lib.rs`:226 on 2024-01-08 22:15_

I think this should be enforced in the build step, not the setup step.

---

_@charliermarsh reviewed on 2024-01-08 22:15_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/distribution_database.rs`:274 on 2024-01-08 22:15_

@konstin -- In this PR, I allow `prepare_metadata_for_build_wheel` even if `--no-build` is specified. Do you feel that's correct?

---

_@charliermarsh reviewed on 2024-01-08 22:16_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/source/mod.rs`:512 on 2024-01-08 22:16_

These methods are structurally similar to (e.g.) `async fn path`, but with the distinction that it will try to use `prepare_metadata_for_build_wheel`. If the build backend doesn't support it, it ends up being identical to calling `path`.

---

_Review requested from @konstin by @charliermarsh on 2024-01-08 22:16_

---

_Label `enhancement` added by @charliermarsh on 2024-01-08 22:16_

---

_Label `performance` added by @charliermarsh on 2024-01-08 22:16_

---

_@konstin reviewed on 2024-01-08 22:24_

---

_Review comment by @konstin on `crates/puffin-distribution/src/distribution_database.rs`:274 on 2024-01-08 22:24_

No, my idea was that `--no-build` doesn't run untrusted code, while `prepare_metadata_for_build_wheel` runs arbitrary code. (Less in a security way and more in a doesn't mess with my system way)

(will review properly tomorrow)

---

_Review requested from @zanieb by @charliermarsh on 2024-01-08 22:45_

---

_@charliermarsh reviewed on 2024-01-08 22:47_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/distribution_database.rs`:274 on 2024-01-08 22:47_

We _might_ want separate flags for this then? Since there seem to be two different intents here.

---

_@charliermarsh reviewed on 2024-01-09 00:13_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/distribution_database.rs`:274 on 2024-01-09 00:13_

Actually I agree with you. I'm gonna revert. It's hard to test now though.

---

_Review comment by @konstin on `crates/puffin-build/src/lib.rs`:403 on 2024-01-09 21:53_

```suggestion
        // The method could have been called before, so the directory may already exist.
        fs::create_dir_all(&metadata_directory)?;
```

Feels wrong though, should we remove it before instead? (non-blocking for this PR, we aren't using this)

---

_Review comment by @konstin on `crates/puffin-distribution/src/source/manifest.rs`:12 on 2024-01-09 21:58_

`build_wheel` returns only one wheel, aren't the others from the cache? (I think the code is correct but the comment misleading)

---

_Review comment by @konstin on `crates/puffin-distribution/src/source/mod.rs`:580 on 2024-01-09 22:12_

It's already there, but i think this should be `read_cached_metadata`

---

_Review comment by @konstin on `crates/puffin-distribution/src/source/mod.rs`:972 on 2024-01-09 22:15_

Complaining about my own oversight here:
```suggestion
    #[instrument(skip_all, fields(dist))]
```

---

_Review comment by @konstin on `crates/puffin-distribution/src/source/mod.rs`:367 on 2024-01-09 22:16_

I'd move that check into `build_source_dist_metadata`

---

_@konstin approved on 2024-01-09 22:17_

Do we have a test case for both a backend with `prepare_metadata_for_build_wheel` and without? If the existing test cases cover this we should document it.

Otherwise only very minor comments :shipit: 

---

_@charliermarsh reviewed on 2024-01-09 23:20_

---

_Review comment by @charliermarsh on `crates/puffin-build/src/lib.rs`:403 on 2024-01-09 23:20_

I think you're right that we should use `create_dir` here...

---

_Merged by @charliermarsh on 2024-01-10 00:07_

---

_Closed by @charliermarsh on 2024-01-10 00:07_

---

_Branch deleted on 2024-01-10 00:07_

---
