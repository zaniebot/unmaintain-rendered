```yaml
number: 15118
title: "Add a hint to use `extra-build-dependencies` when a build fails with a missing package"
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - error messages
assignees: []
created_at: 2025-08-06T20:54:18Z
updated_at: 2025-08-20T09:56:43Z
url: https://github.com/astral-sh/uv/issues/15118
synced_at: 2026-01-12T16:02:04Z
```

# Add a hint to use `extra-build-dependencies` when a build fails with a missing package

---

_@zanieb_

e.g., see https://github.com/astral-sh/uv/issues/15039#issuecomment-3161503863

We should add an automatic hint here.

---

_Label `error messages` added by @zanieb on 2025-08-06 20:54_

---

_Label `help wanted` added by @zanieb on 2025-08-06 20:54_

---

_Comment by @blueraft on 2025-08-10 11:20_

We have a similar hint already for `torch`, `wheel`, and `distutils` [here](https://github.com/astral-sh/uv/blob/ed499d7453e5202a269abbf1507cb8cba3c6ee54/crates/uv-build-frontend/src/error.rs#L45-L55). The regex didn't catch the error message from deepspeed.

Should we update the existing [hint](https://github.com/astral-sh/uv/blob/ed499d7453e5202a269abbf1507cb8cba3c6ee54/crates/uv-build-frontend/src/error.rs#L579) to use `extra-build-dependencies` instead? 

---

_Comment by @zanieb on 2025-08-10 16:22_

Sounds good to me. We should include `pip` and `setuptools`? I'm unsure if we should just always hint on module not found instead of having a subset of libraries?

---

_Comment by @blueraft on 2025-08-11 15:42_

> I'm unsure if we should just always hint on module not found instead of having a subset of libraries?

Do we have a list of libraries for this? 🤔

---

_Comment by @zanieb on 2025-08-11 15:45_

Like a mapping of library -> package?

---

_Comment by @blueraft on 2025-08-11 15:51_

Yes

---

_Comment by @zanieb on 2025-08-11 16:21_

We could pick it out of https://github.com/astral-sh/uv/pull/14579

---

_Comment by @blueraft on 2025-08-20 09:28_

This can be closed

---

_Closed by @charliermarsh on 2025-08-20 09:56_

---
