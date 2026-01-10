```yaml
number: 6955
title: "fix: adjust close_handles pointer offsets to match distlib cleanup_fds"
type: pull_request
state: merged
author: samypr100
labels:
  - windows
assignees: []
merged: true
base: main
head: adjust-close-handle-offsets
created_at: 2024-09-03T02:31:08Z
updated_at: 2024-09-04T22:44:30Z
url: https://github.com/astral-sh/uv/pull/6955
synced_at: 2026-01-10T12:53:37Z
```

# fix: adjust close_handles pointer offsets to match distlib cleanup_fds

---

_Pull request opened by @samypr100 on 2024-09-03 02:31_

## Summary

Resolves issues mentioned in comments
* https://github.com/astral-sh/uv/issues/6699#issuecomment-2322515962
* https://github.com/astral-sh/uv/issues/6866#issuecomment-2322785906

Further investigation on the comments revealed that the pointer arithmethic being performed in `let handle_start = unsafe { crt_magic.offset(1 + handle_count) };` from [posy trampoline](https://github.com/njsmith/posy/blob/dda22e6f90f5fefa339b869dd2bbe107f5b48448/src/trampolines/windows-trampolines/posy-trampoline/src/bounce.rs#L146) had some slight errors.  Since `crt_magic` was a `*const u32`, doing an offset by `1 + handle_count` would offset by too much, with some possible out of bounds reads or attempts to call CloseHandle on garbage.

We needed to offset differently since we want to offset by `handle_count` bytes after the initial offset as seen in [launcher.c](https://github.com/pypa/distlib/blob/888c48b56886b03398646be1217508830427bd75/PC/launcher.c#L578). Similarly, we needed to skip the first 3 handles, otherwise we'd still be attempting to close standard I/O handles of the parent (in this case the shell from `busybox.exe sh -l`).

I also added a few extra checks available from `launcher.c` which checks if the handle value is `-2` just to match the distlib implementation more closely and minimize differences.

## Test Plan

Manually compiled distlib's launcher with additional logging and replaced `Lib/site-packages/pip/_vendor/distlib/t64.exe` with the compiled one to log pointers. As a result, I was able to verify the retrieved handle memory addresses in this function actually match in both uv and distlib's implementation from within busybox.exe nested shell where this behavior can be observed and manually tested.

I was also able to confirm this fixes the issues mentioned in the comments, at least with busybox's shell, but I assume this would fix the case with cmake.

## Open areas

`launcher.c` also [checks the size](https://github.com/pypa/distlib/blob/888c48b56886b03398646be1217508830427bd75/PC/launcher.c#L573-L576) of `cbReserved2` before retrieving `handle_start` which this function currently doesn't do. If we wanted to, we could add the additional check here as well, but I wasn't fully sure why it wasn't added in the first place. Thoughts?

```rust
// Verify the buffer is large enough
if si.cbReserved2 < (size_of::<u32>() as isize + handle_count + size_of::<HANDLE>() as isize * handle_count) as u16 {
    return;
}
```

---

_Comment by @samypr100 on 2024-09-03 14:25_

CC @konstin 

---

_Assigned to @konstin by @konstin on 2024-09-03 14:27_

---

_Label `windows` added by @konstin on 2024-09-03 14:27_

---

_@konstin approved on 2024-09-04 11:31_

---

_Merged by @konstin on 2024-09-04 11:31_

---

_Closed by @konstin on 2024-09-04 11:31_

---

_Branch deleted on 2024-09-04 22:44_

---
