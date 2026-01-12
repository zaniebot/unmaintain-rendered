```yaml
number: 2108
title: Include exit code for build failures
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/add-exit-status
created_at: 2024-03-01T11:21:37Z
updated_at: 2024-03-06T01:05:51Z
url: https://github.com/astral-sh/uv/pull/2108
synced_at: 2026-01-12T16:04:52Z
```

# Include exit code for build failures

---

_@konstin_

`uv pip install mysqlclient==2.1.1` on python 3.12 on windows, where the are no binary wheels:

![grafik](https://github.com/astral-sh/uv/assets/6826232/31f6294a-d845-4c85-b663-82a82ae925a6)

Part of #2052.

---

_Label `error messages` added by @konstin on 2024-03-01 11:21_

---

_Comment by @charliermarsh on 2024-03-01 14:43_

I'm finding it hard to tell which exit code belongs to which process, since `exit code: 2` appears twice and `exit code: 1` appears once. How does pip lay it out in this case?

---

_Comment by @konstin on 2024-03-01 15:09_

Sharing linux now, windows after a reboot.

The linux comparison is asymmetric since we never call `egg_info`:

**pip**

![image](https://github.com/astral-sh/uv/assets/6826232/8d456f54-e1d9-4131-971d-bd052da224d5)

**uv**

![image](https://github.com/astral-sh/uv/assets/6826232/5ecd92b6-af42-44ea-a002-a8e23c69bde4)



---

_Comment by @konstin on 2024-03-01 15:26_

## Windows, my IDE terminal

### pip

It's cut off because that's my screen height.

![grafik](https://github.com/astral-sh/uv/assets/6826232/46eaa6eb-1668-48d0-8248-f6a56ed349de)

### uv

![grafik](https://github.com/astral-sh/uv/assets/6826232/34268b0c-011f-4368-9499-99b0ad9889d5)

## Default size windows terminal (powershell)

### pip

![grafik](https://github.com/astral-sh/uv/assets/6826232/07cd984e-2070-407d-8b72-93ec0b03304b)

### uv

![grafik](https://github.com/astral-sh/uv/assets/6826232/8071a007-bda6-417b-bfcc-f3cff023f2eb)

## Full height windows terminal (powershell)

This time a side-by-side view

![grafik](https://github.com/astral-sh/uv/assets/6826232/203cc422-7d56-4ebe-ae93-7e8313746074)



---

_Comment by @konstin on 2024-03-01 15:28_

I'm thinking about adding a message at the bottom after the stdout/stderr so it's immediately clear what failed even if setuptools and compilers spill so much output that it scrolled multiple pages.

---

_Review requested from @charliermarsh by @konstin on 2024-03-04 10:11_

---

_@charliermarsh approved on 2024-03-06 00:49_

---

_Merged by @charliermarsh on 2024-03-06 01:05_

---

_Closed by @charliermarsh on 2024-03-06 01:05_

---

_Branch deleted on 2024-03-06 01:05_

---
