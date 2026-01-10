```yaml
number: 5627
title: "PEP-710: Implement provenance records for installed packages"
type: issue
state: closed
author: fridex
labels:
  - question
  - compatibility
assignees: []
created_at: 2024-07-30T18:52:15Z
updated_at: 2024-08-02T19:33:27Z
url: https://github.com/astral-sh/uv/issues/5627
synced_at: 2026-01-10T04:53:49Z
```

# PEP-710: Implement provenance records for installed packages

---

_Issue opened by @fridex on 2024-07-30 18:52_

We are discussing the possibility of introducing ``provenance_url.json`` that would help with tracking of installed packages in Python environments. It could help with creating a lock files or SBOMs from installed packages.

We would like to know your position on this PEP (it's in draft state as of today) as uv is one of the affected installers. See https://peps.python.org/pep-0710/ and this [Discourse thread](https://discuss.python.org/t/pep-710-recording-the-provenance-of-installed-packages/25428) where the discussion around this PEP is happening. Thanks!

---

_Comment by @zanieb on 2024-07-30 18:52_

Thank you!

---

_Label `question` added by @zanieb on 2024-07-30 18:53_

---

_Label `compatibility` added by @zanieb on 2024-07-30 18:53_

---

_Comment by @charliermarsh on 2024-07-31 21:22_

Thank you -- replied on the thread (https://discuss.python.org/t/pep-710-recording-the-provenance-of-installed-packages/25428/34), feel free to ping me directly on the thread if you have further questions.

---

_Closed by @charliermarsh on 2024-07-31 21:22_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-31 21:22_

---

_Comment by @kdeldycke on 2024-08-02 19:33_

@fridex I you're interested about creating an SBOM from a uv environment, check my [Meta Package Manager](https://github.com/kdeldycke/meta-package-manager) CLI and this comment: https://github.com/astral-sh/uv/issues/4711#issuecomment-2266020335

---
