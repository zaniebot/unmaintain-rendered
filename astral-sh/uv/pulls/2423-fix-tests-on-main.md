```yaml
number: 2423
title: Fix tests on main
type: pull_request
state: merged
author: konstin
labels:
  - testing
assignees: []
merged: true
base: main
head: konsti/fix-tests-protobuf
created_at: 2024-03-13T19:22:37Z
updated_at: 2024-03-13T19:32:49Z
url: https://github.com/astral-sh/uv/pull/2423
synced_at: 2026-01-12T16:05:02Z
```

# Fix tests on main

---

_@konstin_

A new protobuf release on pypi broke our tests.

This is the same version that pip installs:

```console
$ pip install hashb_foxglove_protocolbuffers_python==25.3.0.1.20240226043130+465630478360 --extra-index-url https://buf.build/gen/python
  Looking in indexes: https://pypi.org/simple, https://buf.build/gen/python
  Collecting hashb_foxglove_protocolbuffers_python==25.3.0.1.20240226043130+465630478360
    Downloading https://buf.build/gen/python/hashb-foxglove-protocolbuffers-python/hashb_foxglove_protocolbuffers_python-25.3.0.1.20240226043130%2B465630478360-py3-none-any.whl
       - 34.1 kB 1.9 MB/s 0:00:00
  Collecting protobuf (from hashb_foxglove_protocolbuffers_python==25.3.0.1.20240226043130+465630478360)
    Downloading protobuf-5.26.0-cp37-abi3-manylinux2014_x86_64.whl.metadata (592 bytes)
  Downloading protobuf-5.26.0-cp37-abi3-manylinux2014_x86_64.whl (302 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 302.8/302.8 kB 2.8 MB/s eta 0:00:00
  Installing collected packages: protobuf, hashb_foxglove_protocolbuffers_python
  Successfully installed hashb_foxglove_protocolbuffers_python-25.3.0.1.20240226043130+465630478360 protobuf-5.26.0
```

I added a constraints file for future releases of protobuf.

---

_Label `testing` added by @charliermarsh on 2024-03-13 19:27_

---

_Merged by @charliermarsh on 2024-03-13 19:32_

---

_Closed by @charliermarsh on 2024-03-13 19:32_

---

_Branch deleted on 2024-03-13 19:32_

---
