---
number: 194
title: Create docker container for testing with common build requirements
type: issue
state: closed
author: konstin
labels: []
assignees: []
created_at: 2023-10-26T11:39:21Z
updated_at: 2023-11-02T11:03:57Z
url: https://github.com/astral-sh/uv/issues/194
synced_at: 2026-01-10T01:23:03Z
---

# Create docker container for testing with common build requirements

---

_Issue opened by @konstin on 2023-10-26 11:39_

Add a container to run resolution, source distribution building and other kinds of testing in, which all need to build source distributions. This container serves three purposes:
* Isolation from potentially malicious packages (see https://moyix.blogspot.com/2022/09/someones-been-messing-with-my-subnormals.html and related tweets - many of these are less malicious and more writing stuff where they shouldn't)
* Having common requirements installed: build-essentials, cmake, myql headers, postgres headers, cmake, rust, etc.
* Reproducibility between developers. E.g. i just had a failure due to lack of cmake which i have installed on other machines.

---

_Comment by @konstin on 2023-10-27 12:38_

I had https://pypi.org/project/nvidia-pyindex/ modify my local pip config so i'll do this before any further testing

---

_Assigned to @konstin by @konstin on 2023-10-27 12:38_

---

_Referenced in [astral-sh/uv#238](../../astral-sh/uv/pulls/238.md) on 2023-10-31 11:33_

---

_Closed by @konstin on 2023-11-02 11:03_

---
