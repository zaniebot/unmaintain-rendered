```yaml
number: 12292
title: "uv build: add --keep-on-error"
type: pull_request
state: open
author: da-x
labels:
  - enhancement
assignees: []
base: main
head: keep-on-error
created_at: 2025-03-18T16:51:41Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/12292
synced_at: 2026-01-12T16:10:12Z
```

# uv build: add --keep-on-error

---

_@da-x_

While debugging build failures, it is sometimes useful to look at the build directory for logs. Let's allow not deleting it.

---

_Comment by @zanieb on 2025-03-18 17:57_

Should this just be the default behavior?

---

_Review requested from @konstin by @zanieb on 2025-03-18 17:57_

---

_Label `enhancement` added by @konstin on 2025-03-19 13:01_

---

_Comment by @konstin on 2025-03-19 13:03_

This is a neat idea! Assuming that the operating system reliably cleans up those directories on reboot, I'd actually prefer preserving temporary directories on build failure unconditionally.

---

_Comment by @da-x on 2025-03-19 13:15_

I don't think we should change the default, as this applies to the `~/.cache/uv/build-v0` directory which the OS does not clean up.

---

_Comment by @konstin on 2025-03-19 14:02_

Makes sense!

---

_Comment by @zanieb on 2025-03-19 14:07_

We could clean it with `uv cache clean` or `uv cache prune` though and we could overwrite it on a subsequent build so there's only a single build directory for a given project version. I'm sort of partial to retaining information on error unless there are major concerns about disk use?

---

_Review request for @konstin removed by @konstin on 2025-03-27 21:55_

---
