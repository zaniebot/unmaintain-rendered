```yaml
number: 6863
title: Show env option in CLI reference documentation
type: pull_request
state: merged
author: eth3lbert
labels:
  - documentation
assignees: []
merged: true
base: main
head: doc-reference-env
created_at: 2024-08-30T12:55:52Z
updated_at: 2024-09-03T17:12:16Z
url: https://github.com/astral-sh/uv/pull/6863
synced_at: 2026-01-12T16:07:33Z
```

# Show env option in CLI reference documentation

---

_@eth3lbert_

## Summary

Closes #6469.

<img width="721" alt="image" src="https://github.com/user-attachments/assets/be144e43-e02f-473e-921c-91cf00c3c8d3">



---

_Review comment by @zanieb on `crates/uv-dev/src/generate_cli_reference.rs`:276 on 2024-09-03 13:50_

<img width="836" alt="Screenshot 2024-09-03 at 8 49 18 AM" src="https://github.com/user-attachments/assets/09c29271-2adb-4beb-97db-8e14e9af9bb4">

It feels a little weird that it's not styled (e.g. with code blocks?) and the `[env: ...]` format makes a little less sense for the documentation. I'm not quite sure what I want, but are you willing to play with the style a bit? Otherwise I can get to that eventually.

---

_@zanieb reviewed on 2024-09-03 13:50_

---

_Assigned to @zanieb by @zanieb on 2024-09-03 13:50_

---

_Label `documentation` added by @zanieb on 2024-09-03 13:50_

---

_Review comment by @eth3lbert on `crates/uv-dev/src/generate_cli_reference.rs`:276 on 2024-09-03 14:10_

sure! what about:
<img width="613" alt="image" src="https://github.com/user-attachments/assets/4113d0e7-e291-4fbb-aaf8-cf1fc2186dc5">



---

_@eth3lbert reviewed on 2024-09-03 14:10_

---

_@eth3lbert reviewed on 2024-09-03 14:40_

---

_Review comment by @eth3lbert on `crates/uv-dev/src/generate_cli_reference.rs`:276 on 2024-09-03 14:40_

some random styles 🙈 
<img width="735" alt="image" src="https://github.com/user-attachments/assets/00a8b9a7-b684-4fbd-a7c0-57d529f9e9b9">


---

_@zanieb reviewed on 2024-09-03 16:19_

---

_Review comment by @zanieb on `crates/uv-dev/src/generate_cli_reference.rs`:276 on 2024-09-03 16:19_

Maybe just "May also be set with the `{NAME}` environment variable."?

---

_@eth3lbert reviewed on 2024-09-03 16:26_

---

_Review comment by @eth3lbert on `crates/uv-dev/src/generate_cli_reference.rs`:276 on 2024-09-03 16:26_

Like this?
<img width="720" alt="image" src="https://github.com/user-attachments/assets/3ca3ef2e-db8f-483c-8257-d1bcb4621a25">


---

_@zanieb reviewed on 2024-09-03 17:10_

---

_Review comment by @zanieb on `crates/uv-dev/src/generate_cli_reference.rs`:276 on 2024-09-03 17:10_

Looks reasonable. We can always iterate on this. Thanks!

---

_@zanieb approved on 2024-09-03 17:10_

---

_Merged by @zanieb on 2024-09-03 17:10_

---

_Closed by @zanieb on 2024-09-03 17:10_

---

_Branch deleted on 2024-09-03 17:12_

---
