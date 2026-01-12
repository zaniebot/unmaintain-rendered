```yaml
number: 11618
title: Run uv-dev tests
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/uv-dev-tests
created_at: 2025-02-19T12:24:24Z
updated_at: 2025-02-20T22:28:11Z
url: https://github.com/astral-sh/uv/pull/11618
synced_at: 2026-01-12T16:09:55Z
```

# Run uv-dev tests

---

_@konstin_

In #6827, we switched the uv-dev binary to not being built by default. As an unintended side effect, we were also stopping to run the tests that ensured the schema was up-to-date.

To fix this, we split uv-dev into an unconditional library, with only the binary being a conditional build. This way, `cargo test` and `cargo nextest` pick those tests up again.

An alternative would be running tests with the `dev` feature, with the side effect of always building the uv-dev binary, too.


---

_Label `internal` added by @konstin on 2025-02-19 12:24_

---

_Merged by @charliermarsh on 2025-02-20 22:28_

---

_Closed by @charliermarsh on 2025-02-20 22:28_

---

_Branch deleted on 2025-02-20 22:28_

---
