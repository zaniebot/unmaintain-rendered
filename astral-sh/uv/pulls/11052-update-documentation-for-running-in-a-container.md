```yaml
number: 11052
title: Update documentation for running in a container
type: pull_request
state: merged
author: returnDanilo
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-01-29T08:27:44Z
updated_at: 2025-02-19T09:20:53Z
url: https://github.com/astral-sh/uv/pull/11052
synced_at: 2026-01-10T11:10:34Z
```

# Update documentation for running in a container

---

_Pull request opened by @returnDanilo on 2025-01-29 08:27_

This PR rewords the instructions for using uv in a container. I'm a new user and was somewhat confused by it, so I've rewritten it as I'd have liked to have read it.

It makes it more clear what distroless means, to avoid confusion with other projects that ship OS files with an image with its tag name clear of qualifiers(`astral-sh/uv`, in this case). An example of that is caddy, which ships with alpine.

This also modifies the original example to use a distroful image instead of distroless one while #8635 doesn't get resolved.

---

_Review requested from @zanieb by @charliermarsh on 2025-01-29 14:06_

---

_Label `documentation` added by @charliermarsh on 2025-01-29 14:06_

---

_@zanieb reviewed on 2025-01-29 14:08_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:24 on 2025-01-29 14:08_

We shouldn't use alpine because we can't manage Python versions there yet, perhaps Debian?

---

_Comment by @zanieb on 2025-01-29 14:08_

Thanks! Don't worry about the lint, I can fix that up at merge time.

---

_@zanieb approved on 2025-01-29 22:02_

I made some tweaks, lmk if you have thoughts.

---

_@returnDanilo reviewed on 2025-01-30 00:07_

---

_Review comment by @returnDanilo on `docs/guides/integration/docker.md`:25 on 2025-01-30 00:07_

I'd have removed "basic" from this line, as it's already clear it's a "getting started" type of example. Obvs it doesn't really matter in this one case, but it's a style I'd keep throughout the docs ;)

---

_@zanieb reviewed on 2025-01-30 00:15_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:25 on 2025-01-30 00:15_

Fair!

---

_Merged by @zanieb on 2025-01-30 00:17_

---

_Closed by @zanieb on 2025-01-30 00:17_

---

_Branch deleted on 2025-02-19 09:20_

---
