```yaml
number: 817
title: Improve contributing documentation
type: issue
state: closed
author: zanieb
labels:
  - documentation
assignees: []
created_at: 2024-01-06T16:28:35Z
updated_at: 2024-02-27T12:49:51Z
url: https://github.com/astral-sh/uv/issues/817
synced_at: 2026-01-10T05:40:31Z
```

# Improve contributing documentation

---

_Issue opened by @zanieb on 2024-01-06 16:28_

- Add a link to the README
- Add some basic guidelines to CONTRIBUTING
- Describe the architecture of the project
- Ensure `scripts` projects have README files
- Link to relevant scripts in CONTRIBUTING

---

_Added to milestone `Initial release` by @zanieb on 2024-01-06 16:28_

---

_Label `documentation` added by @charliermarsh on 2024-01-06 20:10_

---

_Comment by @zanieb on 2024-01-10 00:41_

Looks like since #859 we require `cmake` to be installed

---

_Comment by @konstin on 2024-01-10 10:29_

We can recommend install [cmake from pypi](https://pypi.org/project/cmake/), we should also use this for the source dist install of puffin itself by putting it in the build backend requires.

---

_Comment by @charliermarsh on 2024-01-24 15:14_

Should we just make zlib-ng the non-default backend for simplicity?

---

_Comment by @konstin on 2024-01-24 15:27_

I find cmake acceptable since you can `pipx install cmake`.

---

_Comment by @daniel-soutar-ki on 2024-02-22 11:43_

Hi - would really love to contribute to `uv` if at all possible. Any links or suggestions very welcome!

---

_Closed by @konstin on 2024-02-27 12:49_

---
