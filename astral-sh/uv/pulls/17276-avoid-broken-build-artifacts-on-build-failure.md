```yaml
number: 17276
title: Avoid broken build artifacts on build failure
type: pull_request
state: merged
author: bybrooks
labels:
  - bug
assignees: []
merged: true
base: main
head: bybrooks/atomic-build-output
created_at: 2025-12-31T09:05:28Z
updated_at: 2026-01-06T14:27:05Z
url: https://github.com/astral-sh/uv/pull/17276
synced_at: 2026-01-12T16:12:41Z
```

# Avoid broken build artifacts on build failure

---

_@bybrooks_

## Summary

Fixes https://github.com/astral-sh/uv/issues/17232

When `uv build` fails (e.g., due to missing `__init__.py`), partial distribution 
files were being left in the `dist/` directory. 

### Changes

- Use `NamedTempFile` to write build output to a temporary file first
- Only persist the file to the final location on successful build
- If build fails, the temporary file is automatically cleaned up
- Added `Error::Persist` variant for handling persistence failures

## Test Plan

Added `no_partial_files_on_build_failure` test that verifies:

1. `build_source_dist` fails when `__init__.py` is missing
2. `build_wheel` fails when `__init__.py` is missing  
3. The `dist/` directory remains empty after both failures (no partial files)



---

_Review comment by @EliteTK on `crates/uv-build-backend/src/lib.rs`:36 on 2025-12-31 10:06_

I think having a custom error here is probably a good idea, but just a small request:

`tempfile::PersistError` contains both the `io::Error` and the `NamedTempFile` we failed to persist. We're not making use of the whole `NamedTempFile` and don't want to keep it around unnecessarily while we do error handling, we just need the `io::Error` inside.

---

_Review comment by @EliteTK on `crates/uv-build-backend/src/source_dist.rs`:39 on 2025-12-31 10:51_

Rather than `temp_file.as_file().try_clone()?` just make `TarGzWriter::new` accept `&temp_file` (not really fussed between `&'a NamedTempFile` and `writer: W where W: Write`, it's only used by this function).

I don't know what cases `try_clone` would fail in but it's better to just avoid the opportunity entirely.

---

_Review comment by @EliteTK on `crates/uv-build-backend/src/wheel.rs`:56 on 2025-12-31 10:51_

Ditto https://github.com/astral-sh/uv/pull/17276/changes#r2655204555

---

_Review comment by @EliteTK on `crates/uv-build-backend/src/wheel.rs`:307 on 2025-12-31 10:51_

Ditto https://github.com/astral-sh/uv/pull/17276/changes#r2655204555

---

_Review comment by @EliteTK on `crates/uv-build-backend/src/lib.rs`:1082 on 2025-12-31 11:04_

It might be a good idea to also test the case when there are existing files and we are building to overwrite them.

---

_@EliteTK reviewed on 2025-12-31 11:31_

Thanks for this!

I left a few comments, I'm somewhat new here so my colleagues might have additional comments.

It would be good to add an integration test in `crates/uv/tests/it/build_backend.rs` to check the error messages.

---

There's something to consider, this changes the behaviour for handling pre-existing built files. Currently we start by overwriting them. This means that in the case of a failure, the existing files are lost and replaced with incomplete new ones. Now we are only overwriting the files if we succeed. Personally it's what _I_ would expect here. 

But when reporting errors, the user will see something like "Failed to ... dist/project-0.1.0-py2.py3-none-any.whl", implying the specified file was not able to be built, but if they check, they will see there _is_ a file there.

Could this be confusing and should we do something about it? I see a couple of options aside from just leaving it as is:
* Delete any target files before attempting to write the new ones
* Mention in error reporting that the (or we could make it easier for ourselves and just say "any") existing file has not been overwritten

---

_Assigned to @EliteTK by @EliteTK on 2025-12-31 11:32_

---

_@bybrooks reviewed on 2025-12-31 18:27_

---

_Review comment by @bybrooks on `crates/uv-build-backend/src/lib.rs`:36 on 2025-12-31 18:27_

Thank you. I made the changes at c3f88c3e1f7d83a291bcbda70970f15b73166269 .

---

_@bybrooks reviewed on 2025-12-31 18:27_

---

_Review comment by @bybrooks on `crates/uv-build-backend/src/source_dist.rs`:39 on 2025-12-31 18:27_

Thank you. I made the changes at 2bc47a5528da9d9ca5e61cf5bcc2c6b15052593f .

---

_Comment by @bybrooks on 2025-12-31 18:35_

@EliteTK 
Thank you for the feedback!

>Delete any target files before attempting to write the new ones
Mention in error reporting that the (or we could make it easier for ourselves and just say "any") existing file has not been overwritten


I'll go with **Delete any target files before attempting to write the new ones**.

This ensures a consistent state where the existence of the target file directly indicates a successful build.

I'll implement this by deleting the target file (if it exists) before creating the temporary file.

---

_Comment by @EliteTK on 2025-12-31 18:38_

@bybrooks regarding the deleting - feel free, although once we discuss this further, we might not want to go with that option, so also feel free to wait for a verdict there. There is no rush at least on my end.

Main issue is that everyone else is on annual leave :)

---

_Comment by @codspeed-hq[bot] on 2025-12-31 18:47_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/bybrooks%3Abybrooks%2Fatomic-build-output?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #17276 will **not alter performance**

<sub>Comparing <code>bybrooks:bybrooks/atomic-build-output</code> (bc49b6e) with <code>main</code> (82a6a66)</sub>



### Summary

`✅ 5` untouched  





---

_Review requested from @konstin by @zanieb on 2026-01-03 14:02_

---

_@bybrooks reviewed on 2026-01-05 11:26_

---

_Review comment by @bybrooks on `crates/uv-build-backend/src/lib.rs`:1082 on 2026-01-05 11:26_

Thank you. I made the changes at 3b70a1107b121eebd7306b966b655d3db9c43eba .

---

_Comment by @konstin on 2026-01-05 12:07_

> There's something to consider, this changes the behaviour for handling pre-existing built files. Currently we start by overwriting them. This means that in the case of a failure, the existing files are lost and replaced with incomplete new ones. Now we are only overwriting the files if we succeed. Personally it's what _I_ would expect here.
> 
> But when reporting errors, the user will see something like "Failed to ... dist/project-0.1.0-py2.py3-none-any.whl", implying the specified file was not able to be built, but if they check, they will see there _is_ a file there.
> 
> Could this be confusing and should we do something about it? I see a couple of options aside from just leaving it as is:
> 
>     * Delete any target files before attempting to write the new ones
> 
>     * Mention in error reporting that the (or we could make it easier for ourselves and just say "any") existing file has not been overwritten

Deleting the target file(s) before starting the build seems to be the easiest. No strong opinion, users shouldn't rely on a specific the state of `dist/` after a non-zero exist code anyway.

---

_@konstin approved on 2026-01-05 12:30_

---

_Label `bug` added by @konstin on 2026-01-05 12:30_

---

_Comment by @bybrooks on 2026-01-06 12:54_

> Deleting the target file(s) before starting the build seems to be the easiest. No strong opinion, users shouldn't rely on a specific the state of dist/ after a non-zero exist code anyway.

Thanks for the reply. 
I’ve implemented the logic to delete existing files before the build starts. （ bc49b6e6464a97469ba109e0530e30c5936f3545）

---

_Renamed from "Clean up partial sdist/wheel on build failure" to "Avoid broken build artifacts on build failure" by @konstin on 2026-01-06 14:21_

---

_Merged by @konstin on 2026-01-06 14:27_

---

_Closed by @konstin on 2026-01-06 14:27_

---
