---
number: 16714
title: types-protobuf pyi files are not put into venv
type: issue
state: closed
author: trajano
labels:
  - bug
assignees: []
created_at: 2025-11-13T02:04:42Z
updated_at: 2025-11-13T02:16:21Z
url: https://github.com/astral-sh/uv/issues/16714
synced_at: 2026-01-10T01:26:09Z
---

# types-protobuf pyi files are not put into venv

---

_Issue opened by @trajano on 2025-11-13 02:04_

### Summary

I have types-protobuf as a transitive dependency and pyright works.  In VS Code it falls into the `typeshed-fallback` version.

However, in PyCharm, the pyright/typeshed-fallback doesn't work.

Checking out the .venv I don't see the pyi files in `site-packages/google/protobuf` folder.

I tried 

```
uv add --dev types-protobuf
uv add types-protobuf
```

But either way, the files do not appear and PyCharm notes it as a typing error.

### Platform

windows

### Version

uv 0.9.7 (0adb44480 2025-10-30)

### Python version

Python 3.14.0

---

_Label `bug` added by @trajano on 2025-11-13 02:04_

---

_Comment by @trajano on 2025-11-13 02:07_

```
pip install --force-reinstall  types-protobuf # not uv pip
```
Does add
```
.venv/Lib/site-packages/google-stubs/protobuf/timestamp_pb2.pyi
```
With that PyCharm works as expected.

---

_Comment by @charliermarsh on 2025-11-13 02:09_

I can't reproduce this:

```
❯ uv add types-protobuf
Using CPython 3.14.0
Creating virtual environment at: .venv
Resolved 2 packages in 123ms
Prepared 1 package in 89ms
Installed 1 package in 1ms
 + types-protobuf==6.32.1.20251105

❯ ls .venv/lib/python3.14/site-packages/google-stubs/protobuf
__init__.pyi            empty_pb2.pyi           source_context_pb2.pyi
any_pb2.pyi             field_mask_pb2.pyi      struct_pb2.pyi
api_pb2.pyi             internal                symbol_database.pyi
compiler                json_format.pyi         text_format.pyi
descriptor_database.pyi message_factory.pyi     timestamp_pb2.pyi
descriptor_pb2.pyi      message.pyi             type_pb2.pyi
descriptor_pool.pyi     py.typed                util
descriptor.pyi          reflection.pyi          wrappers_pb2.pyi
duration_pb2.pyi        runtime_version.pyi
```

---

_Comment by @trajano on 2025-11-13 02:16_

Weird seems to work after clearing everything.  I also updated to 0.9.9 just now.

---

_Closed by @trajano on 2025-11-13 02:16_

---
