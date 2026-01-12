```yaml
number: 9947
title: build backend - issue with license metadata element
type: issue
state: closed
author: cthoyt
labels:
  - bug
assignees: []
created_at: 2024-12-16T22:24:19Z
updated_at: 2024-12-17T14:20:01Z
url: https://github.com/astral-sh/uv/issues/9947
synced_at: 2026-01-12T16:00:03Z
```

# build backend - issue with license metadata element

---

_@cthoyt_

I'm on uv 0.5.8 (80d41671b 2024-12-11). Update: I also tried this on uv 0.5.9 (0652800cb 2024-12-13) and got the same results.

I created an example project and added a dummy license file to it with:

```console
$ uv --preview init xyz --build-backend uv
$ touch xyz/LICENSE
```

I'm not sure if there's an interface through uv to add metadata to pyproject.toml, but I added the following license declaration under the `[project]` section:

```
license = { file = "LICENSE" }
```

Then, I got the following error with builds:

```console
$ uv --preview build
Building source distribution (uv build backend)...
Building wheel from source distribution (uv build backend)...
  × Failed to build `/Users/cthoyt/Desktop/xyz`
  ╰─▶ failed to open file
      `/Users/cthoyt/Library/Caches/uv/sdists-v6/.tmp5tXXVU/xyz-0.1.0/LICENSE`: No such
      file or directory (os error 2)
```

Interestingly, the following works, which doesn't try building the wheel from the sdist:

```console
$ uv --preview build --sdist --wheel
Building source distribution (uv build backend)...
Building wheel (uv build backend)...
Successfully built dist/xyz-0.1.0.tar.gz
Successfully built dist/xyz-0.1.0-py3-none-any.whl
```

If I trade the license declaration for the following, it works.

```
license-files = [
    "LICENSE"
]
```

However, it's not clear if this is because the `license` element of the metadata is deprecated, or if there's a bug in handling it. I am aware that the metadata versions and the license declarations are currently a sore spot, so thanks for any advice :)


---

_Assigned to @konstin by @zanieb on 2024-12-16 22:26_

---

_Label `bug` added by @konstin on 2024-12-17 10:58_

---

_Comment by @konstin on 2024-12-17 11:05_

We recommend using the PEP 639 `license = "<SPDX expression>"` and `license-files = ["LICENSE"]` declarations, however the old behavior is definitely a bug (#9965)!

---

_Closed by @konstin on 2024-12-17 14:20_

---
