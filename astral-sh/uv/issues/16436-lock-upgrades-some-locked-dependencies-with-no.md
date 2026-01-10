```yaml
number: 16436
title: lock upgrades some locked dependencies with no --upgrade flag
type: issue
state: closed
author: ghost
labels:
  - bug
assignees: []
created_at: 2025-10-24T13:08:59Z
updated_at: 2025-12-16T10:47:52Z
url: https://github.com/astral-sh/uv/issues/16436
synced_at: 2026-01-10T03:11:35Z
```

# lock upgrades some locked dependencies with no --upgrade flag

---

_Issue opened by @ghost on 2025-10-24 13:08_

### Summary

UV updates some packages when locking with no upgrade flag:

Take a pyproject.toml file with two packages.   
Here, requests and an example package:

```toml
[project]
name = "lock-test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [ "requests<=2.29", "example-package<=2.29"]
```
run `uv lock` → both are locked to the matching version 2.29.0
change the pyproject toml to allow higher version (or upload a new package version):
```toml
[project]
name = "lock-test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [ "requests>=2.29", "example-package>=2.29"]
```
run `uv lock`

*expected behavior*: 

lockfile is updated but no dependencies change

*observed behavior*: 
`requests` stays with the locked version, but `example-package` is updated to the latest version.



I have done some investigation and it seems to be related to the uploaded distributions. `example-package` only contains py3-none-any wheel files: `GET $INDEX/simple/example-package` -> 
`example-package-2.29.0-py3-none-any.whl, example-package-2.29.1-py3-none-any.whl`

 This error occurs on any package where this is the case and does not appear as soon as sdist or an additional wheel platform was uploaded.

### Platform

macOS26

### Version

0.9.4 

### Python version

3.10-3.13

---

_Label `bug` added by @ghost on 2025-10-24 13:09_

---

_Assigned to @konstin by @zanieb on 2025-10-24 14:22_

---

_Comment by @charliermarsh on 2025-10-28 23:43_

Can you please create an example that we can run against PyPI?

---

_Label `bug` removed by @charliermarsh on 2025-10-28 23:43_

---

_Label `needs-mre` added by @charliermarsh on 2025-10-28 23:43_

---

_Comment by @ghost on 2025-10-31 08:44_

I found a package that only contains the py3-none-any wheels but to my surprise, it does not cause the same issue. So this may be caused by artifactory. I'll do some more debugging to see if I can reproduce it more reliably

---

_Comment by @ghost on 2025-11-07 10:48_

I think I now found the actual condition for reproducing it. It only occurs when authentication is used.
I reproduced it on both artifactory and gitlab package registries.  If I take an open index and set `authenticate = "always"`, the package is updated. When I change that to `authenticate = "never"` it is not.

I used the package `12factor-vault` here because that meets the "only py3-none-any.whl" condition on pypi.org. I'd expect this to happen with any package thats deployed with `uv build --wheel` and `twine upload dist/*`

---

_Unassigned @konstin by @konstin on 2025-11-30 19:09_

---

_Label `needs-mre` removed by @konstin on 2025-12-16 10:34_

---

_Label `bug` added by @konstin on 2025-12-16 10:34_

---

_Closed by @konstin on 2025-12-16 10:47_

---
