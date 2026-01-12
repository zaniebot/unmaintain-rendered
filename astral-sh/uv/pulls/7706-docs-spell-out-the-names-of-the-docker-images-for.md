```yaml
number: 7706
title: "Docs: Spell out the names of the Docker images for easier copy-paste"
type: pull_request
state: merged
author: akx
labels:
  - documentation
assignees: []
merged: true
base: main
head: docker-images-docs
created_at: 2024-09-26T12:27:28Z
updated_at: 2024-09-26T14:18:47Z
url: https://github.com/astral-sh/uv/pull/7706
synced_at: 2026-01-12T16:07:57Z
```

# Docs: Spell out the names of the Docker images for easier copy-paste

---

_@akx_

## Summary

It was all too easy to just copy the non-qualified name of the Docker images and wonder why they couldn't be found – well, because they're on `ghcr.io`, and you need to read the prose before the list to figure that out.


## Test Plan

No plan.

---

_@zanieb reviewed on 2024-09-26 12:35_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:29 on 2024-09-26 12:35_

Yes, we need to keep this or prettier breaks the rendering of the list which requires an indent of four.

---

_Label `documentation` added by @zanieb on 2024-09-26 12:35_

---

_@akx reviewed on 2024-09-26 12:41_

---

_Review comment by @akx on `docs/guides/integration/docker.md`:29 on 2024-09-26 12:41_

Okay, I'll re-add it. What requires an indent of four for nested lists, though, out of curiosity? (PyCharm's built-in Markdown renderer is fine with two.)

---

_Review requested from @zanieb by @akx on 2024-09-26 13:53_

---

_@zanieb reviewed on 2024-09-26 14:18_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:29 on 2024-09-26 14:18_

mkdocs, I guess. https://github.com/astral-sh/uv/pull/5819

---

_@zanieb approved on 2024-09-26 14:18_

---

_Merged by @zanieb on 2024-09-26 14:18_

---

_Closed by @zanieb on 2024-09-26 14:18_

---
