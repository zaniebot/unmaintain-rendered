---
number: 16020
title: extend the add command for adding the latest version of a package
type: issue
state: open
author: electronic-shop-bartel
labels:
  - enhancement
assignees: []
created_at: 2025-09-25T06:54:04Z
updated_at: 2025-09-25T06:54:04Z
url: https://github.com/astral-sh/uv/issues/16020
synced_at: 2026-01-07T13:12:19-06:00
---

# extend the add command for adding the latest version of a package

---

_Issue opened by @electronic-shop-bartel on 2025-09-25 06:54_

### Summary

UV does a great job when it comes to package management. However the feature missing most when migrating from poetry is adding the latest version of a package and updating the spec inside the pyproject.toml. 
At the moment I have to do a "uv pip install <package> --upgrade" to find out the version and add it manually to the pyproject.toml.
After this it can happen, that when you updated the file and do a "uv sync" I have to solve dependency caused by this new package since my version constrains of subpackages is not always meeting the "new" package requirements. It would be great, if a command like described above would suggest, what packages needs to be updated in order to install the "latest" version of package you want to install and prompt if you want to do this.

### Example

uv add mypackage --latest

installing mypackage==2.0.0
required updates:
my-sub-package>=2.0.0
mysecond-sub-package>=3.0.0
do you want to update? y/N: y


---

_Label `enhancement` added by @electronic-shop-bartel on 2025-09-25 06:54_

---
