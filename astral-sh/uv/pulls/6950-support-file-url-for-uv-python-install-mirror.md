```yaml
number: 6950
title: "Support file:// URL for `UV_PYTHON_INSTALL_MIRROR`"
type: pull_request
state: merged
author: eth3lbert
labels:
  - enhancement
assignees: []
merged: true
base: main
head: python-mirror-file-url
created_at: 2024-09-02T23:10:35Z
updated_at: 2025-09-08T13:05:46Z
url: https://github.com/astral-sh/uv/pull/6950
synced_at: 2026-01-10T06:44:32Z
```

# Support file:// URL for `UV_PYTHON_INSTALL_MIRROR`

---

_Pull request opened by @eth3lbert on 2024-09-02 23:10_

## Summary

Closes #6319.

## Test Plan

I tested with `file:///mirror`, `file://localhost/mirror`, and `http://mirror` to confirm that it was working as expected.

``` shell-session
/private/tmp/mirror-local                                                                                                                                                                      07:08:18
:)  tree mirror 
mirror/
└── 20240814/
   └── cpython-3.12.5+20240814-aarch64-apple-darwin-install_only_stripped.tar.gz
```

<img width="626" alt="image" src="https://github.com/user-attachments/assets/9c04224d-305c-47ee-a524-4a6abeb79da4">



---

_Review requested from @charliermarsh by @charliermarsh on 2024-09-03 00:14_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-03 00:14_

---

_Label `enhancement` added by @charliermarsh on 2024-09-03 00:14_

---

_@charliermarsh approved on 2024-09-03 01:13_

Thanks!

---

_Merged by @charliermarsh on 2024-09-03 01:20_

---

_Closed by @charliermarsh on 2024-09-03 01:20_

---

_Branch deleted on 2024-09-03 03:27_

---

_Comment by @aleksarias on 2025-06-05 20:12_

I've set the environment variable: 
``` bash 
UV_PYTHON_INSTALL_MIRROR='file:///Users/a0274042/PycharmProjects/my-project/python_standalone' 
```
and I get the following error when running: 
``` bash 
uv sync
error: failed to query metadata of file `/Users/a0274042/PycharmProjects/my-project/backend/python_standalone/20250212/cpython-3.13.2+20250212-aarch64-apple-darwin-install_only_stripped.tar.gz`: No such file or directory (os error 2)
```

Is there a reason this is occurring? It looks like uv is finding the file but then is unable to query the metadata... and it's showing an os error for some reason.
The printed path in the error is definitely correct and the tar.gz file lives there. 

---

_Comment by @zanieb on 2025-06-05 22:53_

@aleksarias please open a new issue per #9452 

Maybe include some `ls -lah` of the directory? Maybe it's a permissions problem?

---

_Comment by @hk2281 on 2025-06-11 16:21_

Thanks for the thread — @aleksarias  I ran into the same issue and was able to solve it.

In my case, the local mirror had the following structure:

```
mirror-local/
└── mirror/
    └── 20240814/
        └── cpython-3.13.2+20240814-<platform>.tar.gz
```
However, my Python archive was named with a different date: 20241002.
After I renamed the subdirectory to match the date in the archive filename:
```
mirror-local/
└── mirror/
    └── 20241002/
        └── cpython-3.13.2+20241002-<platform>.tar.gz
```
uv sync worked correctly.

---

_Comment by @leodevian on 2025-09-08 13:05_

I encountered this issue as well. It seems that the mirror that is created using `scripts/create-python-mirror.py` depends on the version of uv as older versions of uv won't request the latest release of python-build-standalone for the same Python versions.

---
