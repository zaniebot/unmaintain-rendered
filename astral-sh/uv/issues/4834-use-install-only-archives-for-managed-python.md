```yaml
number: 4834
title: "Use `install_only` archives for managed Python installations"
type: issue
state: closed
author: zanieb
labels:
  - enhancement
  - needs-decision
assignees: []
created_at: 2024-07-05T15:36:49Z
updated_at: 2024-07-07T02:43:56Z
url: https://github.com/astral-sh/uv/issues/4834
synced_at: 2026-01-12T15:58:52Z
```

# Use `install_only` archives for managed Python installations

---

_@zanieb_

These have a significant size reduction and are automatically prioritized to the optimized version for each platform, see https://github.com/astral-sh/uv/pull/4775#discussion_r1666890324

What are the downsides to using these? Rye does not appear to use them. I don't think we're using any of the build metadata.

---

_Label `enhancement` added by @zanieb on 2024-07-05 15:37_

---

_Label `needs-decision` added by @zanieb on 2024-07-05 15:37_

---

_Comment by @j178 on 2024-07-05 18:50_

Rye added support for toolchain installation without build info in https://github.com/astral-sh/rye/pull/917. It still fetches the`full` version and then renames the inner `install` directory to the final target, discarding the build info in the process. I think it is possible for uv to use an `install-only` archive, thereby reducing the fetch size.

---

_Comment by @charliermarsh on 2024-07-05 19:57_

This makes sense.

---

_Comment by @charliermarsh on 2024-07-05 20:36_

Although it seems like you can't get PGO and/or LTO with `install-only`.

---

_Comment by @charliermarsh on 2024-07-05 20:42_

Oh nevermind, I misunderstood. They're already optimized. Let's use them!

---

_Closed by @charliermarsh on 2024-07-07 02:43_

---

_Closed by @charliermarsh on 2024-07-07 02:43_

---
