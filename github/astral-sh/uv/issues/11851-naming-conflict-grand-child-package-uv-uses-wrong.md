---
number: 11851
title: Naming conflict grand-child package - UV uses wrong index
type: issue
state: open
author: PedroMTQ
labels:
  - question
assignees: []
created_at: 2025-02-28T13:07:54Z
updated_at: 2025-02-28T14:11:14Z
url: https://github.com/astral-sh/uv/issues/11851
synced_at: 2026-01-07T13:12:18-06:00
---

# Naming conflict grand-child package - UV uses wrong index

---

_Issue opened by @PedroMTQ on 2025-02-28 13:07_

### Summary

I'm trying to install an internal package (pkg1) that depends on another internal package (pkg2), which I can do so by adding the respective azure uv.index.
However, I'm having an issue where pkg2 depends on a package (pkg3) that is available internally and publicly (naming conflict).
This then causes issues when installing pkg1, since UV refers to the publicly available package, instead of the one defined in the pkg2 pyproject.toml. If I don't define the pkg3 index in the pkg1 pyproject, then UV will install the pkg3 from the wrong index (public package), for example:

![Image](https://github.com/user-attachments/assets/57469c50-1f4c-4e94-9e6f-67fcdcba8f03)



If I define the pkg3 as a dependency of pkg1, then the installation works correctly, for example: 

![Image](https://github.com/user-attachments/assets/501baa88-f616-4ee8-aee8-c33db2a05e0a)

However, for obvious reasons, I would prefer not to have this grand-child dependency explicitly managed, and would prefer if pkg2 manages it (to avoid having to update pkg3 dependency on both pkg1 and pkg2)

### Platform

Ubuntu 20.04.6 LTS

### Version

0.6.2

### Python version

3.11

---

_Label `bug` added by @PedroMTQ on 2025-02-28 13:07_

---

_Comment by @konstin on 2025-02-28 13:31_

Would it work for uv if you define the azure index as the default index and pypi as second index? This way, all packages available on the internal index would use the internal index, and only packages not available there would use pypi.

---

_Comment by @charliermarsh on 2025-02-28 14:11_

Yeah I think your only options here (IIUC) are: (1) define `pkg3` as a dependency so that you can set the `tool.uv.sources` on it; or (2) remove `explicit = true` so that uv prioritizes the Azure index over PyPI.

---

_Label `bug` removed by @charliermarsh on 2025-02-28 14:11_

---

_Label `question` added by @charliermarsh on 2025-02-28 14:11_

---
