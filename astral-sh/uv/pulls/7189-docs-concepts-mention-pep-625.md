```yaml
number: 7189
title: "docs(concepts): mention PEP 625"
type: pull_request
state: merged
author: mkniewallner
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/follow-up-source-distribution
created_at: 2024-09-08T14:59:46Z
updated_at: 2024-09-08T21:47:46Z
url: https://github.com/astral-sh/uv/pull/7189
synced_at: 2026-01-10T12:53:41Z
```

# docs(concepts): mention PEP 625

---

_Pull request opened by @mkniewallner on 2024-09-08 14:59_

## Summary

Follow-up to https://github.com/astral-sh/uv/pull/7168#discussion_r1748914880.

---

_@zanieb reviewed on 2024-09-08 15:02_

---

_Review comment by @zanieb on `docs/concepts/resolution.md`:303 on 2024-09-08 15:02_

```suggestion
[PEP 625](https://peps.python.org/pep-0625/) specifies that packages must distribute source
distributions as gzip tarball (`.tar.gz`) archives. Prior to this specification, other archive formats
were allowed so uv also supports reading and extracting archives in the following formats:
```

---

_@zanieb reviewed on 2024-09-08 15:02_

---

_Review comment by @zanieb on `docs/concepts/resolution.md`:303 on 2024-09-08 15:02_

Maybe something like this?

---

_@mkniewallner reviewed on 2024-09-08 15:04_

---

_Review comment by @mkniewallner on `docs/concepts/resolution.md`:303 on 2024-09-08 15:04_

Woops sorry, I force-pushed before your review, since it was still in draft (damn you're fast! 😄). Will incorporate your suggestions though, thanks!

---

_Marked ready for review by @mkniewallner on 2024-09-08 15:15_

---

_@charliermarsh approved on 2024-09-08 21:28_

Great, thanks!

---

_Label `documentation` added by @charliermarsh on 2024-09-08 21:28_

---

_Merged by @charliermarsh on 2024-09-08 21:28_

---

_Closed by @charliermarsh on 2024-09-08 21:28_

---

_Branch deleted on 2024-09-08 21:47_

---
