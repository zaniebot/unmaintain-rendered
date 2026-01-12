```yaml
number: 14972
title: "Skip`cargo dev generate-all` test case in CI"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/generate-dev
created_at: 2025-07-30T13:46:50Z
updated_at: 2025-07-31T15:49:43Z
url: https://github.com/astral-sh/uv/pull/14972
synced_at: 2026-01-12T16:11:31Z
```

# Skip`cargo dev generate-all` test case in CI

---

_@zanieb_

This means that CI tests fail in a way that is redundant with the dedicated CI job which can obscure signal on whether actual tests are failing

e.g., https://github.com/astral-sh/uv/actions/runs/16623645930/job/47034116533

---

_Label `internal` added by @zanieb on 2025-07-30 13:46_

---

_Renamed from "zb/generate dev" to "Remove test case for `cargo dev generate-all`" by @zanieb on 2025-07-30 13:47_

---

_Comment by @zanieb on 2025-07-30 13:48_

I believe this existed for auto-merge purposes, but... I just added `cargo dev generate-all` to the required jobs instead.

---

_Marked ready for review by @zanieb on 2025-07-30 13:49_

---

_Comment by @konstin on 2025-07-30 13:50_

This test catches an outdated schema when running tests locally. Can we turn this job of for CI instead?

---

_Comment by @zanieb on 2025-07-30 14:01_

I never run this test locally. I feel like you should have a different way to check an outdated schema locally? Like a `cargo dev generate-all --mode check` hook? I don't think this should be the responsibility of the test suite.

---

_Comment by @zanieb on 2025-07-30 14:01_

It's sort of like saying we should be checking that `prettier` passes in the test suite

---

_@charliermarsh approved on 2025-07-30 23:56_

As long as we still enforce this in CI, it's fine with me to remove it as a test.

---

_Renamed from "Remove test case for `cargo dev generate-all`" to "Skip`cargo dev generate-all` test case in CI" by @zanieb on 2025-07-31 13:08_

---

_Comment by @konstin on 2025-07-31 13:15_

When working on Rust code, it's easy to change something that affects the schema, and it's helpful if the test suite catches that. For me, this is very different to prettier, which I know when it's required (different filetype) and I can configure to format-on-save.

---

_@konstin approved on 2025-07-31 13:15_

---

_Merged by @zanieb on 2025-07-31 15:49_

---

_Closed by @zanieb on 2025-07-31 15:49_

---

_Branch deleted on 2025-07-31 15:49_

---
