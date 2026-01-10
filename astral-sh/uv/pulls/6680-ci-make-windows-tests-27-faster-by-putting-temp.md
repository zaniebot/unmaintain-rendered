```yaml
number: 6680
title: "ci: Make Windows tests ~27% faster by putting temp folder in dev drive"
type: pull_request
state: merged
author: fasterthanlime
labels:
  - internal
assignees: []
merged: true
base: main
head: ci-tests
created_at: 2024-08-27T09:30:59Z
updated_at: 2024-09-05T00:32:26Z
url: https://github.com/astral-sh/uv/pull/6680
synced_at: 2026-01-10T12:53:33Z
```

# ci: Make Windows tests ~27% faster by putting temp folder in dev drive

---

_Pull request opened by @fasterthanlime on 2024-08-27 09:30_

## Summary

This PR makes `cargo test | windows` faster in CI.

### Before

![Windows tests take 5m44s](https://github.com/user-attachments/assets/8dd9c619-9b7b-4ebd-a027-56e7967b6d34)

### After

![Windows tests take 5m12s](https://github.com/user-attachments/assets/7702fdba-3034-4db8-b211-85207a1feffa)

## Also

This PR disables the `brotli` feature of `async-compression` since it's not strictly needed, but this has little to do with the improvements (it's still less code to build).

This PR introduces additional code in uv tool uninstall to ignore errors (that only seem to happen on ReFS, ie. on Dev Drives) akin to "the thing we're trying to delete cannot be deleted because it's already being deleted".

If `raw_os_error` was stable we could do u32 matching instead of that `.to_string().contains()` abomination.

---

_Renamed from "WIP: ci tests (Disable brotli for now)" to "ci: Make Windows tests ~27% faster" by @fasterthanlime on 2024-08-27 20:06_

---

_Renamed from "ci: Make Windows tests ~27% faster" to "ci: Make Windows tests ~27% faster by putting temp folder in dev drive" by @fasterthanlime on 2024-08-27 20:08_

---

_Label `internal` added by @zanieb on 2024-08-27 20:23_

---

_@zanieb approved on 2024-08-27 20:24_

Awesome! This looks great.

---

_Merged by @zanieb on 2024-08-27 20:25_

---

_Closed by @zanieb on 2024-08-27 20:25_

---

_Comment by @samypr100 on 2024-08-27 21:25_

@fasterthanlime Curious, it seems `tempfile::tempdir` seems to ultimately leverage [GetTempPath2](https://learn.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-gettemppath2a) which has the following rules:

>For non-system processes, the GetTempPath2 function checks for the existence of environment variables in the following order and uses the first path found:
>The path specified by the TMP environment variable.
>The path specified by the TEMP environment variable.
>The path specified by the USERPROFILE environment variable.
>The Windows directory.

Would this have work if `TEMP` env var was set instead to a directory inside the dev drive location? I wonder if that approach may simplify this by quite a bit.

---

_Comment by @zanieb on 2024-09-05 00:32_

Part of https://github.com/astral-sh/uv/issues/5713

---
