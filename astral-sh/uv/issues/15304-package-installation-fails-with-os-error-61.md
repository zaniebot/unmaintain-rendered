```yaml
number: 15304
title: Package installation fails with os error 61
type: issue
state: open
author: srdecny
labels:
  - bug
  - external
assignees: []
created_at: 2025-08-15T12:45:30Z
updated_at: 2025-10-29T10:00:28Z
url: https://github.com/astral-sh/uv/issues/15304
synced_at: 2026-01-12T16:02:07Z
```

# Package installation fails with os error 61

---

_@srdecny_

### Summary

When installing packages, I get the following error:

```
(env) srdecny@bender7 ~/meaning4 (master)> uv pip install torch torchaudio
Using Python 3.11.8 environment at: env
Resolved 26 packages in 68ms
error: Failed to install: torch-2.8.0-cp311-cp311-manylinux_2_28_x86_64.whl (torch==2.8.0)
  Caused by: failed to copy file from /fsx_home/homes/srdecny/.cache/uv/archive-v0/kow9mkBtE1FIwZCcXJYyP/torch-2.8.0.dist-info/RECORD to /fsx_home/homes/srdecny/meaning4/env/lib/python3.11/site-packages/torch-2.8.0.dist-info/RECORD: No data available (os error 61)
```

The `/fsx_home` is a FSx for Lustre drive hosted on AWS. I assume this is relevant here. It seems to happen with any package, and it persists even if I delete the `~.cache/uv` folder and start with a fresh virtual env. The package it errors out also seems to be different every time. Maybe some race condition here?

### Platform

Linux 6.5.0-1024-aws x86_64 GNU/Linux

### Version

uv 0.7.21

### Python version

3.11.8

---

_Label `bug` added by @srdecny on 2025-08-15 12:45_

---

_Comment by @blaylockbk on 2025-08-26 20:13_

I've had the same issue on a Lustre FS. Usually running `uv cache clean` first fixes the issue. But this isn't ideal as clearing a rebuilding the cache takes time.

---

_Comment by @zanieb on 2025-08-26 20:51_

Interesting thanks for the report.

Does it persist after the initial failure? e.g., you get `No data available (os error 61)` on the same file every time once it occurs? Or do we just need to add retries here?

---

_Comment by @zanieb on 2025-08-26 20:51_

(It'd be great if you could loop in someone from the AWS FSx team as they're likely to have domain knowledge we do not here)

---

_Comment by @bnikolic on 2025-09-17 13:43_

Not to do with *uv* per se, but I've had very similar recurring issue building Rust itself (v1.85) on Amazon FSx/Lustre.  

```
  >> 6281    failed to copy `/shared/fsx1/bntemp/spack/opt/spack/linux-x86_64_v3/rust-bootstrap-1.85.0-llhzwidyz4uj4dekylhf5hdemfc7rezw/lib/
             rustlib/x86_64-unknown-linux-gnu/bin/llvm-ar` to `/tmp/b.nikolic/spack-stage/spack-stage-rust-1.85.0-wf2ccbxnnerjkdihdbjiexxf56
             ifwfzi/spack-src/build/x86_64-unknown-linux-gnu/stage0-sysroot/lib/rustlib/x86_64-unknown-linux-gnu/bin/llvm-ar`: No data avail
             able (os error 61)
  ```

I did not see a similar error in many many hours of compilation of other packages....  Suggests something about how Rust copies file which triggers an unsupported feature or weakness in Lustre implementation of FSx. Perhaps something about how  Lustre uses extended attribute lustre.lov  ...

---

_Label `external` added by @konstin on 2025-09-17 13:56_

---

_Comment by @konstin on 2025-09-17 13:58_

Could you raise this with AWS, e.g. through a support ticket? This seems like a bug in FSx for Lustre, where the AWS team help with, especially when reported by their costumers directly.

---

_Comment by @bnikolic on 2025-09-18 08:52_

I have traced this issue to the use of the sendfile(2) system call to do a file copy without getting the data into user space. Somewhere between the expected semantics in implementations (I've traced issues both to python's shutils and rusts fs::copy) and the actual semantics of FSx there is an issue and ENODATA error code is not handled correctly (by falling back to read()/write() or retrying). 

I'm not going to have a chance to look into this further personally but one of the team here may , I'll post a note here if that happens. In mean time, workaround can be achieved by turning off attempt to sendfile() copy files.

---

_Comment by @bnikolic on 2025-10-29 10:00_

I've made a patch for Python's shutil to fix this issue: https://github.com/python/cpython/pull/139417 . I think this is the best way to deal with this issue, rather than relying on all FS implementations to never return ENODATA .

---
