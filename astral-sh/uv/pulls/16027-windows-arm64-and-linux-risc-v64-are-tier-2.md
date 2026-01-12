```yaml
number: 16027
title: Windows arm64 and Linux RISC-V64 are Tier 2 supported
type: pull_request
state: merged
author: konstin
labels:
  - documentation
assignees: []
merged: true
base: main
head: konsti/winows-arm64-is-supported
created_at: 2025-09-25T16:28:59Z
updated_at: 2025-09-29T15:54:06Z
url: https://github.com/astral-sh/uv/pull/16027
synced_at: 2026-01-12T16:12:04Z
```

# Windows arm64 and Linux RISC-V64 are Tier 2 supported

---

_@konstin_

Windows arm64 and Linux RISC-V64 are supported. Windows arm64 is special because you can also use the x86_64 stack, which may even be a better experience.

---

_Review requested from @geofft by @konstin on 2025-09-25 16:28_

---

_Review requested from @zanieb by @konstin on 2025-09-25 16:28_

---

_Label `documentation` added by @konstin on 2025-09-25 16:29_

---

_@konstin reviewed on 2025-09-25 16:29_

---

_Review comment by @konstin on `docs/reference/policies/platforms.md`:32 on 2025-09-25 16:29_

This is not as well known as e.g. rosetta 2, so I added a sentence. I don't know how much mix and match you can do here.

---

_Renamed from "Windows arm64 and Linux RISC-V64 are supported" to "Windows arm64 and Linux RISC-V64 are Tier 2 supported" by @konstin on 2025-09-25 16:30_

---

_@zanieb reviewed on 2025-09-25 16:31_

---

_Review comment by @zanieb on `docs/reference/policies/platforms.md`:32 on 2025-09-25 16:31_

Did you mean to say "This isn't as well known as ..."?

I'm not sure if we should talk about this here though. It seems out of place? I think it makes more sense in the Python version discussion?

---

_@konstin reviewed on 2025-09-25 16:41_

---

_Review comment by @konstin on `docs/reference/policies/platforms.md`:32 on 2025-09-25 16:41_

What location do you mean?

---

_@zanieb reviewed on 2025-09-25 17:28_

---

_Review comment by @zanieb on `docs/reference/policies/platforms.md`:32 on 2025-09-25 17:28_

Like maybe in https://docs.astral.sh/uv/concepts/python-versions/ ? I'm not sure where would be best. It feels out of place here to talk about Python package support though.

---

_@zanieb approved on 2025-09-29 13:56_

---

_Merged by @konstin on 2025-09-29 15:54_

---

_Closed by @konstin on 2025-09-29 15:54_

---

_Branch deleted on 2025-09-29 15:54_

---
