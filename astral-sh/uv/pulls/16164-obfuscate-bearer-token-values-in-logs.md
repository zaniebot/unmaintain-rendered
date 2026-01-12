```yaml
number: 16164
title: Obfuscate Bearer Token values in logs
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/token-obf
created_at: 2025-10-07T21:27:31Z
updated_at: 2025-10-13T13:28:22Z
url: https://github.com/astral-sh/uv/pull/16164
synced_at: 2026-01-12T16:12:09Z
```

# Obfuscate Bearer Token values in logs

---

_@zanieb_

Sometimes a credential's `Debug` formatted value appears in tracing logs - make sure the credential doesn't appear there.

## Test plan

Added a test case + ran
```
uv pip install --default-index $PYX_API_URL/$SOME_INDEX $SOME_PACKAGE -vv
```
With an authenticated uv client and confirmed the tokens are obfuscated.

---

_Label `enhancement` added by @zanieb on 2025-10-07 21:27_

---

_Comment by @zanieb on 2025-10-07 23:01_

cc @zsol 

---

_Comment by @zsol on 2025-10-09 09:02_

Want me to take over, write tests and ship?

---

_Comment by @zanieb on 2025-10-09 13:28_

Feel free

---

_Marked ready for review by @zsol on 2025-10-13 11:18_

---

_Review requested from @charliermarsh by @zsol on 2025-10-13 11:19_

---

_@zsol approved on 2025-10-13 11:20_

<a href="https://gitme.me/image?url=https%3A%2F%2Fmedia1.giphy.com%2Fmedia%2Fv1.Y2lkPTI5NzJhMmIxZWVwbXF2bGQwNzl4eTJkbG5zeG51djdyZnM4N3hlZmdydmt5dG9pMCZlcD12MV9naWZzX3NlYXJjaCZjdD1n%2Fl36kU80xPf0ojG0Erg%2Fgiphy.gif&token=spiderman" data-gitmeme-token="spiderman"><img src="https://media1.giphy.com/media/v1.Y2lkPTI5NzJhMmIxZWVwbXF2bGQwNzl4eTJkbG5zeG51djdyZnM4N3hlZmdydmt5dG9pMCZlcD12MV9naWZzX3NlYXJjaCZjdD1n/l36kU80xPf0ojG0Erg/giphy.gif" title="Created by gitme.me with /spiderman"
        alt="spiderman"/></a>

---

_Review comment by @konstin on `crates/uv-auth/src/credentials.rs`:103 on 2025-10-13 12:25_

Do we ever expect to write this token to a (plain text) file through serde? Otherwise we can drop the `Serialize` and avoid accidentally writing it out as part of a larger struct.

---

_@konstin approved on 2025-10-13 12:25_

---

_@zsol reviewed on 2025-10-13 12:45_

---

_Review comment by @zsol on `crates/uv-auth/src/credentials.rs`:103 on 2025-10-13 12:45_

Good call, I don't think Serialize is actually used. Dropping

---

_@zanieb reviewed on 2025-10-13 13:27_

---

_Review comment by @zanieb on `crates/uv-auth/src/credentials.rs`:103 on 2025-10-13 13:27_

Yes, we do — in the same way as `Password`. We just don't support bearer authentication outside of pyx yet, but we will in the future.

We can add it back then though.

---

_Merged by @zanieb on 2025-10-13 13:28_

---

_Closed by @zanieb on 2025-10-13 13:28_

---

_Branch deleted on 2025-10-13 13:28_

---
