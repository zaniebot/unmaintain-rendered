```yaml
number: 10362
title: "uv-pep440: adds an explicit test for trailing zeros"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/pep440-add-trailing-zero-test
created_at: 2025-01-07T14:06:20Z
updated_at: 2025-01-07T14:16:24Z
url: https://github.com/astral-sh/uv/pull/10362
synced_at: 2026-01-12T16:09:15Z
```

# uv-pep440: adds an explicit test for trailing zeros

---

_@BurntSushi_

Basically, this explicitly checks that parsing a `1.2.0` into a
`Version` will roundtrip back to a `1.2.0`, and that parsing a `1.2`
will roundtrip back to a `1.2`.

I think this case is included in the other tests in this module, but
this test makes the behavior more clearly intentional I think.

Ref #10345


---

_Review requested from @konstin by @BurntSushi on 2025-01-07 14:06_

---

_@konstin approved on 2025-01-07 14:06_

---

_Merged by @BurntSushi on 2025-01-07 14:16_

---

_Closed by @BurntSushi on 2025-01-07 14:16_

---

_Branch deleted on 2025-01-07 14:16_

---
