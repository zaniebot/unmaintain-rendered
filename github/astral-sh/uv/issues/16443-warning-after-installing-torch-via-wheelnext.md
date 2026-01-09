---
number: 16443
title: Warning after installing torch via wheelnext
type: issue
state: closed
author: intexcor
labels:
  - bug
assignees: []
created_at: 2025-10-24T19:07:57Z
updated_at: 2025-10-28T23:36:14Z
url: https://github.com/astral-sh/uv/issues/16443
synced_at: 2026-01-07T13:12:19-06:00
---

# Warning after installing torch via wheelnext

---

_Issue opened by @intexcor on 2025-10-24 19:07_

### Summary

social_media_analytics) pet1@h200:~/social_media_analytics$ pip install ninja
Collecting ninja
  Using cached ninja-1.13.0-py3-none-manylinux2014_x86_64.manylinux_2_17_x86_64.whl.metadata (5.1 kB)
Using cached ninja-1.13.0-py3-none-manylinux2014_x86_64.manylinux_2_17_x86_64.whl (180 kB)
WARNING: Error parsing dependencies of torch: Expected a marker variable or quoted string
    dpcpp-cpp-rt==2025.2.1; platform_system == 'Linux' and 'intel' in variant_namespaces                                                                                        
                                                                      ^                                                                                                         
Installing collected packages: ninja                                                                                                                                            
Successfully installed ninja-1.13.0

### Platform

Linux 6.8.0-86-generic x86_64 GNU/Linux

### Version

uv-wheelnext 0.9.3

### Python version

3.13

---

_Label `bug` added by @intexcor on 2025-10-24 19:07_

---

_Comment by @zanieb on 2025-10-24 19:50_

Are you reporting a bug with uv for a warning from pip?

pip doesn't support reading new markers added in the wheel next prototype.

---

_Closed by @charliermarsh on 2025-10-28 23:36_

---
