---
number: 15302
title: "Failure with `code 0xC0000005 at address 0x18000c274` on Windows"
type: issue
state: open
author: babeloo
labels:
  - bug
  - external
assignees: []
created_at: 2025-08-15T11:20:56Z
updated_at: 2025-08-15T12:20:15Z
url: https://github.com/astral-sh/uv/issues/15302
synced_at: 2026-01-07T13:12:19-06:00
---

# Failure with `code 0xC0000005 at address 0x18000c274` on Windows

---

_Issue opened by @babeloo on 2025-08-15 11:20_

### Summary

uv sync didn't sync, and send out error
```
$ uv sync                                                                                  
error: unhandled exception in uv, please report a bug:                                                         
code 0xC0000005 at address 0x18000c274
EXCEPTION_ACCESS_VIOLATION reading 0x1ec
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

I then run with  `RUST_BACKTRACE=1`
```
$env:RUST_BACKTRACE=1; uv sync                                                                           
error: unhandled exception in uv, please report a bug:                                                         
code 0xC0000005 at address 0x18000c274
EXCEPTION_ACCESS_VIOLATION reading 0x1f0
rax=0000000000000000 rbx=0000000000000424 rcx=0000000000000424
rdx=0000000000000002 rsi=000003e6780400e8 rdi=00000000000001f0
rsp=0000009ba45a5c30 rbp=0000009ba45a5d60  r8=7ffffffffffffffc
 r9=0000000000000001 r10=0000000000000424 r11=000001f488c8cee8
r12=0000000000000000 r13=000003e678260880 r14=000003e6780400c0
r15=0000000000000000 rip=000000018000c274 eflags=0000000000010246
stack backtrace:
   0:     0x7ff62e015d2d - <unknown>
   1:     0x7ff62e296b27 - <unknown>
   2:     0x7ffcf5b69cf3 - UnhandledExceptionFilter
   3:     0x7ffcf85ea01f - strncpy
   4:     0x7ffcf85a6553 - _C_specific_handler
   5:     0x7ffcf85e6fef - _chkstk
   6:     0x7ffcf84f4557 - RtlWow64GetCurrentCpuArea
   7:     0x7ffcf85e692e - KiUserExceptionDispatcher
   8:        0x18000c274 - AuthenticateLicense
   9:        0x18001f4c2 - AuthenticateLicense
  10:        0x18000c506 - AuthenticateLicense
  11:     0x7ff62dd0ea5b - <unknown>
  12:     0x7ff62e079212 - <unknown>
  13:     0x7ff62deb479b - <unknown>
  14:     0x7ff62deb9a74 - <unknown>
  15:     0x7ff62def690b - <unknown>
  16:     0x7ff62df0f50b - <unknown>
  17:     0x7ff62df10c61 - <unknown>
  18:     0x7ff62dec0ea7 - <unknown>
  19:     0x7ff62de57bf1 - <unknown>
  20:     0x7ff62ded3dc1 - <unknown>
  21:     0x7ff62dee9234 - <unknown>
  22:     0x7ff62df3ae1e - <unknown>
  23:     0x7ff62f402e49 - <unknown>
  24:     0x7ff62f7a9b55 - <unknown>
  25:     0x7ff62e6d313b - <unknown>
  26:     0x7ff62e6d2cd4 - <unknown>
  27:     0x7ff62ecf7035 - <unknown>
  28:     0x7ff62ed53ac4 - <unknown>
  29:     0x7ff62ed459ca - <unknown>
  30:     0x7ff62f10e72f - <unknown>
  31:     0x7ff62edbaae9 - <unknown>
  32:     0x7ff62f3f33cf - <unknown>
  33:     0x7ff62e3a2c54 - <unknown>
  34:     0x7ff62e3a02f9 - <unknown>
  35:     0x7ff62e5f4420 - <unknown>
  36:     0x7ff62e5ba44c - <unknown>
  37:     0x7ff62e5b0cd6 - <unknown>
  38:     0x7ff62e4d8413 - <unknown>
  39:     0x7ff62e3f9cda - <unknown>
  40:     0x7ff62e3f8fef - <unknown>
  41:     0x7ff62efd5771 - <unknown>
  42:     0x7ff62e8d7980 - <unknown>
  43:     0x7ff62e02966d - <unknown>
  44:     0x7ffcf783e8d7 - BaseThreadInitThunk
  45:     0x7ffcf84bc34c - RtlUserThreadStart
```



### Platform

Windows 11 x86_64

### Version

uv 0.8.11 (f892276ac 2025-08-14)

### Python version

Python 3.12.9

---

_Label `bug` added by @babeloo on 2025-08-15 11:20_

---

_Comment by @zanieb on 2025-08-15 12:18_

Thank you! That looks like an instance of https://github.com/astral-sh/uv/issues/14563

---

_Renamed from "uv sync fail, and uv ask me to report a bug" to "Failure with `code 0xC0000005 at address 0x18000c274` on Windows" by @zanieb on 2025-08-15 12:20_

---

_Label `external` added by @zanieb on 2025-08-15 12:20_

---
