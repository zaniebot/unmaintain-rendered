```yaml
number: 6792
title: "fix: adjust trampoline close_handles invalid to be safer"
type: pull_request
state: merged
author: samypr100
labels:
  - windows
assignees: []
merged: true
base: main
head: adjust-close-handles
created_at: 2024-08-29T04:38:37Z
updated_at: 2024-08-30T13:55:51Z
url: https://github.com/astral-sh/uv/pull/6792
synced_at: 2026-01-12T16:07:32Z
```

# fix: adjust trampoline close_handles invalid to be safer

---

_@samypr100_

## Summary

Closes https://github.com/astral-sh/uv/issues/6699

On cases like the ones described in https://github.com/astral-sh/uv/issues/6699, `lpReserved2` somehow seems to report multiple file descriptors that were not tied to any valid handles. The previous implementation was faulting as it would try to dereference these invalid handles. This change moves to using `HANDLE` directly and check if its is_invalid instead before attempting to close them.

## Test Plan

Manually tested and verified using `busybox-w32` like described in the issue.


---

_Renamed from "feat: adjust trampoline close_handles invalid handle handling" to "fix: adjust trampoline close_handles invalid handle handling" by @samypr100 on 2024-08-29 05:07_

---

_Renamed from "fix: adjust trampoline close_handles invalid handle handling" to "fix: adjust trampoline close_handles invalid to be safer" by @samypr100 on 2024-08-29 05:07_

---

_Review requested from @konstin by @charliermarsh on 2024-08-29 15:54_

---

_Label `windows` added by @charliermarsh on 2024-08-29 15:54_

---

_Comment by @petermbauer on 2024-08-30 07:05_

I wanted to check if this fixes #6464 but it crashes for me with a Stack Overflow. Used the Windows binary produced by the CI (https://github.com/astral-sh/uv/actions/runs/10608916295/artifacts/1868167042 from https://github.com/astral-sh/uv/actions/runs/10608916295/job/29422114640?pr=6792).

```
λ uv --version
uv 0.4.0 (d2f70e3b2 2024-08-29)

λ uv venv .venv --python  C:\Users\bauepete\AppData\Local\Programs\Python\Python310\python.exe
Using Python 3.10.9 interpreter at: C:\Users\bauepete\AppData\Local\Programs\Python\Python310\python.exe
Creating virtualenv at: .venv
Activate with: .venv\Scripts\activate

λ uv pip install --python .venv cmake
⠙ cmake==3.30.2
thread 'main' has overflowed its stack
```

---

_@konstin approved on 2024-08-30 08:49_

---

_Comment by @konstin on 2024-08-30 08:49_

@petermbauer For builds in debug mode, you need to set `$Env:UV_STACK_SIZE = '2000000'`

---

_Merged by @konstin on 2024-08-30 08:55_

---

_Closed by @konstin on 2024-08-30 08:55_

---

_Comment by @petermbauer on 2024-08-30 11:24_

> @petermbauer For builds in debug mode, you need to set `$Env:UV_STACK_SIZE = '2000000'`

thx! `uv pip install` works with this but it seems the issue has not been fixed. I will add repro steps to #6464.

---

_Comment by @samypr100 on 2024-08-30 13:37_

> > @petermbauer For builds in debug mode, you need to set `$Env:UV_STACK_SIZE = '2000000'`
> 
> thx! `uv pip install` works with this but it seems the issue has not been fixed. I will add repro steps to #6464.

A repro for the separate case in #6464 would be appreciated, thanks.

---

_Branch deleted on 2024-08-30 13:38_

---

_Comment by @petermbauer on 2024-08-30 13:55_

> A repro for the separate case in #6464 would be appreciated, thanks.

Thanks a lot for looking into it!
Created #6866 and added repro instructions there as it may be a different reason after all.


---
