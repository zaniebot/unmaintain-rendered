```yaml
number: 11035
title: Add SECURITY policy
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/sec
created_at: 2025-01-28T19:17:07Z
updated_at: 2025-01-28T20:06:56Z
url: https://github.com/astral-sh/uv/pull/11035
synced_at: 2026-01-12T16:09:38Z
```

# Add SECURITY policy

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/11020

---

_Label `documentation` added by @zanieb on 2025-01-28 19:17_

---

_@zanieb reviewed on 2025-01-28 19:17_

---

_Review comment by @zanieb on `SECURITY.md`:18 on 2025-01-28 19:17_

Not live yet.

---

_@zanieb reviewed on 2025-01-28 19:17_

---

_Review comment by @zanieb on `SECURITY.md`:11 on 2025-01-28 19:17_

I'm sure I'm missing things here

---

_@zanieb reviewed on 2025-01-28 19:45_

---

_Review comment by @zanieb on `SECURITY.md`:18 on 2025-01-28 19:45_

Live now.

---

_Marked ready for review by @zanieb on 2025-01-28 19:45_

---

_Comment by @zanieb on 2025-01-28 19:46_

I think we should also outline our process for reporting CVEs, which I think GitHub provides an okay interface for? But I haven't used it and we haven't had to report any CVEs yet so I'm not sure what our process will be.

---

_Review requested from @charliermarsh by @zanieb on 2025-01-28 19:46_

---

_Review requested from @konstin by @zanieb on 2025-01-28 19:46_

---

_@charliermarsh reviewed on 2025-01-28 19:51_

---

_Review comment by @charliermarsh on `SECURITY.md`:5 on 2025-01-28 19:51_

"the Python" seems off. "Due to the design of Python packaging ecosystem"?

---

_@charliermarsh approved on 2025-01-28 19:51_

---

_Review comment by @charliermarsh on `SECURITY.md`:11 on 2025-01-28 19:52_

We could perhaps just mention things like `no-build`?

---

_@charliermarsh reviewed on 2025-01-28 19:52_

---

_@zanieb reviewed on 2025-01-28 19:52_

---

_Review comment by @zanieb on `SECURITY.md`:5 on 2025-01-28 19:52_

Ah, edits.

---

_@zanieb reviewed on 2025-01-28 19:53_

---

_Review comment by @zanieb on `SECURITY.md`:11 on 2025-01-28 19:53_

I was thinking about that but wasn't sure how to do so without being verbose.. let me see what I can do

---

_@zanieb reviewed on 2025-01-28 19:54_

---

_Review comment by @zanieb on `SECURITY.md`:11 on 2025-01-28 19:54_

That sort of belongs in a "Hardening" document rather than the security policy document?

Maybe once that exists we can just link there?

---

_Comment by @zanieb on 2025-01-28 19:54_

I also took a look at https://www.python.org/dev/security/

---

_@konstin approved on 2025-01-28 19:55_

Can we inline this file somewhere else? We already have a lot of top level files.

---

_Comment by @zanieb on 2025-01-28 20:04_

@konstin I don't think so, I think this is the GitHub standard location.

---

_Comment by @zanieb on 2025-01-28 20:04_

We could link to another document (e.g., as they do in pypa/pip) but we need the file.

---

_Merged by @zanieb on 2025-01-28 20:06_

---

_Closed by @zanieb on 2025-01-28 20:06_

---

_Branch deleted on 2025-01-28 20:06_

---
