```yaml
number: 3253
title: "Make `Requirement` generic over url type"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/url-type-parameter
created_at: 2024-04-24T20:38:04Z
updated_at: 2024-05-07T14:45:50Z
url: https://github.com/astral-sh/uv/pull/3253
synced_at: 2026-01-10T14:37:54Z
```

# Make `Requirement` generic over url type

---

_Pull request opened by @konstin on 2024-04-24 20:38_

This change allows switching out the url type for requirements. The original idea was to allow different types for different requirement origins, so that core metadata reads can ban non-pep 508 requirements while we only allow them for requirements.txt. This didn't work out because we expect `&Requirement`s from all sources to match.

I also tried to split `pep508_rs` into a PEP 508 compliant crate and into our extensions, but they are to tightly coupled.

I think this change is an improvement still as it reduces the hardcoded dependence on `VerbatimUrl`.

---

_Label `internal` added by @konstin on 2024-04-24 20:38_

---

_Review comment by @konstin on `crates/uv/tests/pip_compile.rs`:6515 on 2024-04-24 20:39_

I'm not sure about those but they seem fine?

---

_@konstin reviewed on 2024-04-24 20:39_

---

_Renamed from "Parameterize `Requirement` by url type" to "Make `Requirement` generic over url type" by @konstin on 2024-05-06 10:18_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-05-06 13:31_

---

_@charliermarsh reviewed on 2024-05-06 14:02_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:6515 on 2024-05-06 14:02_

How did it happen? Do you understand the change?

---

_@charliermarsh reviewed on 2024-05-07 13:48_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:6515 on 2024-05-07 13:48_

Do you know what the issue was? It looks like it was reverted?

---

_@konstin reviewed on 2024-05-07 14:10_

---

_Review comment by @konstin on `crates/uv/tests/pip_compile.rs`:6515 on 2024-05-07 14:10_

Sorry, forgot to reply: Yes, i found the reason and reverted

---

_@charliermarsh approved on 2024-05-07 14:44_

---

_Merged by @konstin on 2024-05-07 14:45_

---

_Closed by @konstin on 2024-05-07 14:45_

---

_Branch deleted on 2024-05-07 14:45_

---
