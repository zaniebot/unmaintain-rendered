```yaml
number: 10699
title: "Suggest `MACOSX_DEPLOYMENT_TARGET` in resolver platform tag hint"
type: issue
state: open
author: zanieb
labels:
  - help wanted
  - error messages
assignees: []
created_at: 2025-01-16T22:53:02Z
updated_at: 2025-05-27T03:53:19Z
url: https://github.com/astral-sh/uv/issues/10699
synced_at: 2026-01-10T03:41:46Z
```

# Suggest `MACOSX_DEPLOYMENT_TARGET` in resolver platform tag hint

---

_Issue opened by @zanieb on 2025-01-16 22:53_

See https://github.com/astral-sh/uv/issues/10696#issuecomment-2597052942

We should suggest changing the `MACOSX_DEPLOYMENT_TARGET` version when the "best" tag differs from one of the available tags by OS version.

For implementation, see

- https://github.com/astral-sh/uv/pull/10582
https://github.com/astral-sh/uv/blob/b46c6db317b0b534b8ae8d636d041b7779921fa8/crates/uv-resolver/src/pubgrub/report.rs#L1666


---

_Label `error messages` added by @zanieb on 2025-01-16 22:53_

---

_Label `help wanted` added by @zanieb on 2025-01-16 22:53_

---

_Comment by @ywh555hhh on 2025-05-27 03:53_

Do you still need help with this? If so, I'd be happy to pick it up.

---
