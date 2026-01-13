```yaml
number: 17241
title: "fix: handle Windows special filenames in cache clean"
type: pull_request
state: open
author: blueberrycongee
labels:
  - windows
assignees: []
base: main
head: fix/windows-special-filename-cache-clean
created_at: 2025-12-26T18:49:00Z
updated_at: 2026-01-13T21:37:05Z
url: https://github.com/astral-sh/uv/pull/17241
synced_at: 2026-01-13T22:36:21Z
```

# fix: handle Windows special filenames in cache clean

---

_@blueberrycongee_

## Summary

Fix `uv cache clean` failing on Windows when cached sdist contains files with Windows-incompatible filenames (e.g., files ending with a period like `logging.`).

**Problem:**
On Windows, when running `uv add uwsgi`, the build fails (expected, as uwsgi doesn't support Windows). However, when subsequently running `uv cache clean`, the command fails with:

```
error: Failed to clear cache at: AppData\Local\uv\cache
  Caused by: failed to remove file `...\logging.`: The system cannot find the file specified. (os error 2)
```

This happens because Windows' standard Win32 API automatically normalizes paths, stripping trailing dots from filenames. When trying to delete such files, the normalized path doesn't match the actual filename on disk.

**Solution:**
Use the extended-length path prefix (`\\?\`) to bypass Win32 path normalization when deletion fails with `NotFound` or `InvalidInput` errors. This is the standard Windows approach for handling:
- Filenames with special characters (trailing dots, spaces)
- Paths exceeding MAX_PATH limit

The fix is applied to `remove_file`, `remove_dir`, and `remove_dir_all` functions in `crates/uv-cache/src/removal.rs`.

Fixes #16586

## Test Plan

- Added unit tests for `to_extended_path()` function covering:
  - Absolute paths → extended path conversion
  - Already extended paths → returned as-is
  - UNC paths → extended UNC format conversion
  - Non-Windows platforms → no-op behavior

- Added unit tests for file/directory removal:
  - Normal file removal
  - Readonly file removal (verifies `set_not_readonly` logic)
  - Empty directory removal
  - Directory with contents removal (`remove_dir_all`)

- Added tests for `rm_rf()` function:
  - Single file removal with statistics verification
  - Directory tree removal with statistics verification
  - Nonexistent path handling (should succeed with zero counts)

- Added test for `Removal` struct's `AddAssign` implementation

- Manual verification: Reproduced the original issue with `uwsgi` package and confirmed `uv cache clean` now succeeds after the fix.

---

_Review comment by @samypr100 on `crates/uv-cache/src/removal.rs`:230 on 2025-12-29 03:25_

I think an explicit match to Prefix::Verbatim, Prefix::VerbatimDisk, Prefix::VerbatimUNC would be better here. I assume `\\?` is a typo and you meant `\\?\`? Nonetheless, the std::path::Prefix enum would help with this.

---

_Review comment by @samypr100 on `crates/uv-cache/src/removal.rs`:223 on 2025-12-29 03:27_

`to_verbatim_path` would be more appropriate naming wise

---

_Review comment by @samypr100 on `crates/uv-cache/src/removal.rs`:240 on 2025-12-29 03:29_

I'm not convinced we should try to guess what the absolute path should be here as it depends on the context.

---

_Review comment by @samypr100 on `crates/uv-cache/src/removal.rs`:250 on 2025-12-29 03:31_

I'd rather check Prefix::UNC first before handling `\\`

---

_Review comment by @samypr100 on `crates/uv-cache/src/removal.rs`:259 on 2025-12-29 03:32_

Is this needed? All the changed code I see is behind `#[cfg(windows)]`

---

_Review comment by @samypr100 on `crates/uv-cache/src/removal.rs`:309 on 2025-12-29 03:46_

I'd add a test in `crates/uv/tests/it/cache_clean.rs` for the OP scenario instead.

---

_@samypr100 reviewed on 2025-12-29 03:49_

---

_Comment by @blueberrycongee on 2025-12-29 09:04_

The CI failure is unrelated to this PR - it's a GitHub LFS service timeout:

> batch response: Fatal error: We couldn't respond to your request in time.

The failing tests (`tool_install_git_lfs`, `tool_run_git_lfs`) are flaky due to external service issues. Could you please re-run the failed jobs?


---

_Comment by @blueberrycongee on 2025-12-29 09:04_

@samypr100 

---

_Label `windows` added by @samypr100 on 2025-12-31 18:48_

---

_@samypr100 reviewed on 2025-12-31 21:34_

---

_Review comment by @samypr100 on `crates/uv-cache/src/removal.rs`:261 on 2025-12-31 21:34_

Thanks for the applying the changes.

I'm thinking this fits in uv-fs, but I'd like to hear from others first.

CC @konstin 

---

_@samypr100 reviewed on 2025-12-31 23:50_

---

_Review comment by @samypr100 on `crates/uv-cache/src/removal.rs`:225 on 2025-12-31 23:50_

nit: you can return the match directly, and avoid all the other inner returns

---

_@samypr100 reviewed on 2025-12-31 23:56_

---

_Review comment by @samypr100 on `crates/uv-cache/src/removal.rs`:270 on 2025-12-31 23:56_

We should avoid the `#cfg(windows)` here by adding `if cfg!(windows)`. That will help with the possible dead code warnings and make sure we always exercise the compiler on this for all platforms (since there should be no issues)

---

_Review comment by @samypr100 on `crates/uv-cache/src/removal.rs`:289 on 2026-01-02 02:54_

I'm also thinking calling remove_file (recurse once) with a good exit condition here would be better than repeating all the code twice. Same for remove_dir/remove_dir_all.

---

_@samypr100 reviewed on 2026-01-02 02:54_

---

_Comment by @blueberrycongee on 2026-01-05 12:09_

@samypr100 

---

_@konstin reviewed on 2026-01-05 12:19_

---

_Review comment by @konstin on `crates/uv-cache/src/removal.rs`:261 on 2026-01-05 12:19_

Yes this should be a generic util in uv-fs

---

_Comment by @konstin on 2026-01-05 12:32_

I'm not sure about the general approach here, can we do a directory traversal with UNC paths directly instead, to avoid an error occurring in the first place?

---

_@samypr100 reviewed on 2026-01-05 14:01_

---

_Review comment by @samypr100 on `crates/uv-cache/src/removal.rs`:240 on 2026-01-05 14:01_

We'll want to avoid lossy conversion here

---

_Comment by @blueberrycongee on 2026-01-07 09:06_

@samypr100 - Fixed! The UNC path handling now uses PathBuf::push() directly with the &OsStr values instead of to_string_lossy().

@konstin - The current approach falls back to verbatim paths only when needed (on NotFound/InvalidInput errors). This avoids the overhead of always using verbatim paths for directory traversal while still handling edge cases with special characters like trailing dots.

---

_Comment by @konstin on 2026-01-07 09:09_

> This avoids the overhead of always using verbatim paths for directory traversal

What is the overhead for doing that?

---

_Comment by @blueberrycongee on 2026-01-07 12:23_

@konstin Looking at the code, the overhead would be:

Memory allocation: to_verbatim_path() creates new PathBuf/OsString for paths that need conversion (UNC and Disk paths), while already-verbatim paths return Cow::Borrowed with no allocation.

Path component processing: For UNC paths, it does path.components().skip(1).collect() which iterates through the remaining path components after the UNC prefix.

Directory traversal: The current code uses walkdir::WalkDir::new(path) which wouldn't automatically benefit from verbatim paths anyway - we'd need to convert every path during traversal.

The current approach only pays this cost when we hit specific errors (NotFound/InvalidInput), rather than for every path operation.

---

_Comment by @konstin on 2026-01-07 12:29_

Did you use an LLM for that or did you check the options doing a walkdir with verbatim paths yourself?

---

_Comment by @blueberrycongee on 2026-01-07 13:04_

@konstin You're right, I didn't actually test the verbatim-path-first approach before answering.

I just tested it now:

WalkDir::new("\\?\C:\...") works correctly
entry.path() automatically inherits the \\?\ prefix
Deletion with entry.path() succeeds without any fallback logic
So converting to verbatim path once at the rm_rf entry point is cleaner. I can refactor to that approach if you'd prefer.

---

_Comment by @samypr100 on 2026-01-07 17:02_

> @konstin You're right, I didn't actually test the verbatim-path-first approach before answering.
> 
> I just tested it now:
> 
> WalkDir::new("\?\C:...") works correctly entry.path() automatically inherits the \?\ prefix Deletion with entry.path() succeeds without any fallback logic So converting to verbatim path once at the rm_rf entry point is cleaner. I can refactor to that approach if you'd prefer.

Yes, please do so

---

_Assigned to @samypr100 by @zanieb on 2026-01-13 21:37_

---

_Assigned to @konstin by @zanieb on 2026-01-13 21:37_

---
