---
number: 3262
title: uv pip install fails on network drive
type: issue
state: closed
author: BenediktMaag
labels:
  - bug
assignees: []
created_at: 2024-04-25T11:04:00Z
updated_at: 2024-05-08T06:34:50Z
url: https://github.com/astral-sh/uv/issues/3262
synced_at: 2026-01-10T01:23:26Z
---

# uv pip install fails on network drive

---

_Issue opened by @BenediktMaag on 2024-04-25 11:04_

I'm developing a plugin called pymat built with maturin and deployed as a wheel to a (company) network drive. The folder contains all versions of the plugin and im trying to install the most recent one. This works in pip as expected. When trying to install it with uv (Windows, 0.1.38 (0b23caa18 2024-04-24)), uv panics. Currently two wheels are in the folder, 0.1.0 and 0.2.0.

I invoke with the following command:
```
uv pip install --no-index --upgrade --find-links "file:\\some.network.net\path\to\folder\pymat" pymat --verbose
```
```
INFO Found a virtualenv through VIRTUAL_ENV at: C:\git_work\some_project\.venv
DEBUG Cached interpreter info for Python 3.12.0, skipping probing: .venv\Scripts\python.exe
DEBUG Using Python 3.12.0 environment at .venv\Scripts\python.exe
DEBUG Trying to lock if free: .venv\.lock
DEBUG Using registry request timeout of 30s
DEBUG Found 2 packages in `--find-links` entry: \\some.network.net\path\to\folder\pymat
DEBUG Solving with target Python version 3.12.0
DEBUG Adding direct dependency: pymat*
DEBUG Searching for a compatible version of pymat (*)
DEBUG Selecting: pymat==0.2.0 (pymat-0.2.0-cp312-none-win_amd64.whl)
DEBUG Tried 2 versions: pymat 1, root 1
Resolved 1 package in 67ms
thread 'main' panicked at crates\cache-key\src\canonical_url.rs:35:32:
called `Result::unwrap()` on an `Err` value: ()
stack backtrace:
   0:     0x7ff614e944d2 - git_odb_object_data
   1:     0x7ff614ebdc2d - git_odb_object_data
   2:     0x7ff614e8e091 - git_odb_object_data
   3:     0x7ff614e942fa - git_odb_object_data
   4:     0x7ff614e97009 - git_odb_object_data
   5:     0x7ff614e96cc5 - git_odb_object_data
   6:     0x7ff614e97524 - git_odb_object_data
   7:     0x7ff614e973f9 - git_odb_object_data
   8:     0x7ff614e94dd9 - git_odb_object_data
   9:     0x7ff614e970c6 - git_odb_object_data
  10:     0x7ff61508c747 - git_midx_writer_new
  11:     0x7ff61508cc83 - git_midx_writer_new
  12:     0x7ff614c10439 - git_filter_source_repo
  13:     0x7ff614a12f27 - git_filter_source_repo
  14:     0x7ff6148b3207 - git_filter_source_repo
  15:     0x7ff61489ec32 - git_filter_source_repo
  16:     0x7ff61489e8b3 - git_filter_source_repo
  17:     0x7ff61479fc4b - git_filter_source_repo
  18:     0x7ff6145ce9d5 - git_filter_source_repo
  19:     0x7ff61460cb25 - git_filter_source_repo
  20:     0x7ff6145ac3d0 - git_filter_source_repo
  21:     0x7ff6142b34a5 - git_filter_source_repo
  22:     0x7ff61401897b - <unknown>
  23:     0x7ff6140a795f - git_filter_source_repo
  24:     0x7ff614090316 - <unknown>
  25:     0x7ff614e846d2 - git_odb_object_data
  26:     0x7ff6140a847c - git_filter_source_repo
  27:     0x7ff61502e6ec - git_midx_writer_new
  28:     0x7ffba7a37344 - BaseThreadInitThunk
  29:     0x7ffba99226b1 - RtlUserThreadStart
```
Further information:
- When using the mounted drive G: instead of file:\\some.network.net\fs\ the same error occurs.
- When copying the folder to C:\temp\pymat I can install with uv pip install --no-index --upgrade --find-links C:\temp\pymat pymat resulting in the following instead of the panic:
  Installed 1 package in 41ms
   + pymat==0.2.0
- When the plugin is already installed, auditioning works and doesnt crash uv.



---

_Label `bug` added by @charliermarsh on 2024-04-25 13:04_

---

_Referenced in [astral-sh/uv#3306](../../astral-sh/uv/pulls/3306.md) on 2024-04-29 11:07_

---

_Comment by @konstin on 2024-04-29 11:08_

I don't have a network mount to test myself, but could you try https://github.com/astral-sh/uv/pull/3306? This should remove the panic

---

_Comment by @BenediktMaag on 2024-04-29 16:19_

Sure, will try tomorrow. Do i Need to build from source or can i access the executable from the ci somewhere?

---

_Comment by @zanieb on 2024-04-29 16:33_

You'd need to build from source, but we release really often so it may just be out soon.

---

_Comment by @BenediktMaag on 2024-04-30 05:33_

Since it seems my companys "threat detection" keeps deleting uv-trampoline-x86_64-gui.exe I'll check with the next release and report back.

---

_Comment by @BenediktMaag on 2024-05-08 06:34_

Tried with release 0.1.41 and worked like expected. Thanks alot for the fix and keep up the great work :)

---

_Closed by @BenediktMaag on 2024-05-08 06:34_

---
