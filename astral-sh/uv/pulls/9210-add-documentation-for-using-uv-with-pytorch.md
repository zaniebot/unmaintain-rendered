```yaml
number: 9210
title: Add documentation for using uv with PyTorch
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/pytorch
created_at: 2024-11-18T19:21:27Z
updated_at: 2024-11-19T11:51:54Z
url: https://github.com/astral-sh/uv/pull/9210
synced_at: 2026-01-12T16:08:42Z
```

# Add documentation for using uv with PyTorch

---

_@charliermarsh_

## Summary

Now that we have all the pieces in place, this PR adds some dedicated documentation to enable a variety of PyTorch setups.

This PR is downstream of #6523 and builds on the content in there; #6523 will merge first, and this PR will follow.


---

_Label `documentation` added by @charliermarsh on 2024-11-18 19:21_

---

_Marked ready for review by @charliermarsh on 2024-11-18 19:22_

---

_@zanieb reviewed on 2024-11-18 20:45_

---

_Review comment by @zanieb on `docs/guides/integration/pytorch.md`:44 on 2024-11-18 20:45_

Needs indent

---

_@zanieb reviewed on 2024-11-18 20:47_

---

_Review comment by @zanieb on `docs/guides/index.md`:18 on 2024-11-18 20:47_

Add to the `integration/index.md` too

---

_@zanieb reviewed on 2024-11-18 20:48_

---

_Review comment by @zanieb on `docs/guides/integration/pytorch.md`:311 on 2024-11-18 20:48_

Needs indent

---

_@zanieb reviewed on 2024-11-18 20:48_

---

_Review comment by @zanieb on `docs/guides/integration/pytorch.md`:108 on 2024-11-18 20:48_

Nit: We don't really use the "we will" voice

```suggestion
Next, update the `pyproject.toml` to point `torch` and `torchvision` to the desired index:
```

---

_@zanieb approved on 2024-11-18 20:49_

---

_Comment by @UnknownPlatypus on 2024-11-18 21:45_

This is awesome, thanks a lot :raised_hands: 

---

_Merged by @charliermarsh on 2024-11-19 01:09_

---

_Closed by @charliermarsh on 2024-11-19 01:09_

---

_Branch deleted on 2024-11-19 01:09_

---

_@cthoyt reviewed on 2024-11-19 09:03_

---

_Review comment by @cthoyt on `mkdocs.template.yml`:118 on 2024-11-19 09:03_

Looks like this is a duplicate, see below the fastapi line

---

_@YoniChechik reviewed on 2024-11-19 11:51_

---

_Review comment by @YoniChechik on `mkdocs.template.yml`:118 on 2024-11-19 11:51_

fixed in #9220 

---
