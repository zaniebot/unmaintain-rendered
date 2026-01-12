```yaml
number: 2798
title: "`uv pip install` and `uv pip sync` both perform each uninstall twice causing the second one to fail"
type: issue
state: closed
author: Daverball
labels:
  - bug
assignees: []
created_at: 2024-04-03T07:05:47Z
updated_at: 2024-04-12T21:08:58Z
url: https://github.com/astral-sh/uv/issues/2798
synced_at: 2026-01-12T15:58:40Z
```

# `uv pip install` and `uv pip sync` both perform each uninstall twice causing the second one to fail

---

_@Daverball_

I'm running into some really strange behavior where running `uv pip install` causes each package to be uninstalled twice, causing a missing RECORD file error on the second uninstall. Considering I'm not passing `--reinstall` it shouldn't even do a single uninstall.

As far as I can tell it doesn't cause any real issues and is still a lot faster than pip this way, but it is very noisy.

I'm on Linux x86_64 (OpenSUSE Tumbleweed) with uv 0.1.28

`uv pip install uv --verbose`
<details><summary>Details</summary>
<p>

```
INFO Found a virtualenv through VIRTUAL_ENV at: /home/david/git/ocqms/venv
DEBUG Cached interpreter info for Python 3.10.14, skipping probing: venv/bin/python
DEBUG Using Python 3.10.14 environment at venv/bin/python
DEBUG Using registry request timeout of 300s
DEBUG Solving with target Python version 3.10.14
DEBUG Adding direct dependency: uv*
DEBUG Found fresh response for: https://pypi.org/simple/uv/
DEBUG Ignoring installed versions of uv: multiple distributions found
DEBUG Searching for a compatible version of uv (*)
DEBUG Ignoring installed versions of uv: multiple distributions found
DEBUG Selecting: uv==0.1.28 (uv-0.1.28-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/bf/23/e65d0f8cd4d9904a82a616c265bb30ccc802b16226b964e9f668da26dc2b/uv-0.1.28-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
Resolved 1 package in 2ms
DEBUG Preserving seed package: pip==23.0.1
DEBUG Preserving seed package: setuptools==69.2.0
DEBUG Preserving seed package: pip==23.0.1
DEBUG Preserving seed package: setuptools==69.2.0
DEBUG Uninstalled uv (11 files, 3 directories)
warning: Failed to uninstall package at venv/lib64/python3.10/site-packages/uv-0.1.28.dist-info due to missing RECORD file. Installation may result in an incomplete environment.
Installed 1 package in 63ms
 - uv==0.1.28
 - uv==0.1.28
 + uv==0.1.28
``` 

</p>
</details> 

To me it looks like uv gets confused by there being both a `lib` and `lib64` folder, the latter of which is a symbolic link to the former. As soon as I remove the symbolic link `uv` starts to behave normally. Having `lib64` as an alias for `lib` inside virtual environments is an OpenSUSE thing as far as I am aware.

---

_Comment by @zanieb on 2024-04-03 14:16_

Oh interesting. Thanks for raising!

---

_Label `bug` added by @zanieb on 2024-04-03 14:16_

---

_Assigned to @zanieb by @zanieb on 2024-04-03 14:16_

---

_Comment by @charliermarsh on 2024-04-03 14:17_

@zanieb - Perhaps when we iterate over `site_packages`, we filter out symlinks, not just duplicate paths...?

---

_Comment by @zanieb on 2024-04-03 14:44_

@Daverball I put up https://github.com/astral-sh/uv/pull/2806 if you want to give it a try — I'm having a hard time constructing a test case.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-11 22:09_

---

_Unassigned @zanieb by @charliermarsh on 2024-04-11 22:09_

---

_Comment by @charliermarsh on 2024-04-11 22:09_

I was able to reproduce in a container.

---

_Comment by @charliermarsh on 2024-04-11 23:54_

Fixed (with a test) in https://github.com/astral-sh/uv/pull/3002.

---

_Closed by @charliermarsh on 2024-04-12 21:08_

---
