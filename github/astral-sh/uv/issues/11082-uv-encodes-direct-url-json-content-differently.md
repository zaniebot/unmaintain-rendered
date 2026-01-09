---
number: 11082
title: "uv encodes `direct_url.json` content differently than pip"
type: issue
state: closed
author: tringwald
labels:
  - bug
  - compatibility
assignees: []
created_at: 2025-01-29T20:16:13Z
updated_at: 2025-01-31T20:45:50Z
url: https://github.com/astral-sh/uv/issues/11082
synced_at: 2026-01-07T13:12:18-06:00
---

# uv encodes `direct_url.json` content differently than pip

---

_Issue opened by @tringwald on 2025-01-29 20:16_

### Summary

It seems like `pip` and `uv` disagree on whether or not the file path in `direct_url.json` should be urlencoded.

Steps to reproduce:

1. Download any wheel that contains a local version specifier in its name (I'm using `torch-2.5.1+cpu.cxx11.abi-cp39-cp39-linux_x86_64.whl` from [here](https://download.pytorch.org/whl/cpu-cxx11-abi/torch-2.5.1%2Bcpu.cxx11.abi-cp39-cp39-linux_x86_64.whl))
2. `pip install --no-deps torch-2.5.1+cpu.cxx11.abi-cp39-cp39-linux_x86_64.whl`
3. File `.../lib/python3.9/site-packages/torch-2.5.1+cpu.cxx11.abi.dist-info/direct_url.json` now contains (note the `%2B`):
```
{
  "archive_info": {}, 
  "url": "file:///.../torch-2.5.1%2Bcpu.cxx11.abi-cp39-cp39-linux_x86_64.whl"
  #                              ^^^
}
``` 
5. `uv pip install --no-deps torch-2.5.1+cpu.cxx11.abi-cp39-cp39-linux_x86_64.whl`
```
Resolved 1 package in 5ms
Prepared 1 package in 0.11ms
Uninstalled 1 package in 154ms
Installed 1 package in 67ms
 - torch==2.5.1+cpu.cxx11.abi (from file:///.../torch-2.5.1%2Bcpu.cxx11.abi-cp39-cp39-linux_x86_64.whl)
 + torch==2.5.1+cpu.cxx11.abi (from file:///.../torch-2.5.1+cpu.cxx11.abi-cp39-cp39-linux_x86_64.whl)
```
5. The `direct_url.json` file now contains (note the `+`):
```
{
  "archive_info":{},
  "url": "file:///.../torch-2.5.1+cpu.cxx11.abi-cp39-cp39-linux_x86_64.whl"
  #                              ^
}
```



### Platform

Linux

### Version

0.5.25

### Python version

3.9.7

---

_Label `bug` added by @tringwald on 2025-01-29 20:16_

---

_Comment by @charliermarsh on 2025-01-29 20:25_

I believe both versions are perfectly valid per the spec.

---

_Label `compatibility` added by @zanieb on 2025-01-29 20:49_

---

_Comment by @tringwald on 2025-01-29 22:30_

Thanks for the quick response. Another problem here is the diff showing 1 package being uninstalled and 1 package being reinstalled. Manually editing the file to contain a `+` leads to the correct ` ~ torch==...` entry.

```
Resolved 1 package in 5ms
Prepared 1 package in 0.11ms
Uninstalled 1 package in 154ms
Installed 1 package in 67ms
 - torch==2.5.1+cpu.cxx11.abi (from file:///.../torch-2.5.1%2Bcpu.cxx11.abi-cp39-cp39-linux_x86_64.whl)
 + torch==2.5.1+cpu.cxx11.abi (from file:///.../torch-2.5.1+cpu.cxx11.abi-cp39-cp39-linux_x86_64.whl)
```

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-30 03:14_

---

_Referenced in [astral-sh/uv#11088](../../astral-sh/uv/pulls/11088.md) on 2025-01-30 03:44_

---

_Closed by @charliermarsh on 2025-01-31 20:45_

---

_Referenced in [pypa/hatch#1924](../../pypa/hatch/pulls/1924.md) on 2025-03-04 04:00_

---
