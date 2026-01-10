---
number: 1288
title: "Consider changing wording around \"novel\" features"
type: issue
state: closed
author: zanieb
labels:
  - documentation
assignees: []
created_at: 2024-02-12T20:01:54Z
updated_at: 2024-02-17T02:12:05Z
url: https://github.com/astral-sh/uv/issues/1288
synced_at: 2026-01-10T01:23:05Z
---

# Consider changing wording around "novel" features

---

_Issue opened by @zanieb on 2024-02-12 20:01_

The README includes a note about novel features; we should verify these features are not available for other Python package managers and reconsider the wording.

_Originally posted by @zanieb in https://github.com/astral-sh/puffin/pull/1265#discussion_r1486706975_
            

---

_Label `documentation` added by @zanieb on 2024-02-12 20:01_

---

_Added to milestone `Initial release` by @zanieb on 2024-02-12 20:02_

---

_Comment by @pawamoy on 2024-02-15 21:32_

> Novel features such as [dependency version overrides](https://github.com/astral-sh/uv#dependency-overrides) and [alternative resolution strategies](https://github.com/astral-sh/uv#resolution-strategy)

Indeed, both features are supported by PDM:

- https://pdm-project.org/latest/usage/config/#override-the-resolved-package-versions
- https://pdm-project.org/latest/usage/dependency/#lock-strategies

---

_Comment by @charliermarsh on 2024-02-15 21:35_

Awesome, thank you! We'll revise. We did look around but not sure how we missed that.

---

_Comment by @charliermarsh on 2024-02-17 02:12_

This was removed!

---

_Closed by @charliermarsh on 2024-02-17 02:12_

---
