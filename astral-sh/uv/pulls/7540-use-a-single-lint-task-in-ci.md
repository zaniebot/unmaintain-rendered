```yaml
number: 7540
title: Use a single lint task in CI
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/single-lint
created_at: 2024-09-19T10:05:02Z
updated_at: 2024-09-19T11:32:10Z
url: https://github.com/astral-sh/uv/pull/7540
synced_at: 2026-01-12T16:07:52Z
```

# Use a single lint task in CI

---

_@konstin_

We initially had two jobs for fast lints, one for Python and one for Rust (rustfmt). We now have fast lints for more (Rust, Python, {json5,yaml,yml}, Markdown, Readme). Instead of arbitrarily splitting the new cases between Python or Rust and reporting e.g. Prettier failures as Rust problems, we run them in a single job that's still <1min on a regular runner. This also reduces our large test matrix by one job.

Note: Before merging, we need to change the required checks configuration in the settings.

---

_Label `internal` added by @konstin on 2024-09-19 10:05_

---

_@zanieb reviewed on 2024-09-19 10:55_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:62 on 2024-09-19 10:55_

Should we use our action here?

---

_@konstin reviewed on 2024-09-19 11:09_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:62 on 2024-09-19 11:09_

yeah!

---

_@zanieb approved on 2024-09-19 11:10_

🥳 sorry didn't realize we weren't already using it elsewhere

---

_Comment by @zanieb on 2024-09-19 11:12_

I disabled the branch protection check for 'cargo fmt'

---

_Comment by @zanieb on 2024-09-19 11:18_

I think you'll need to disable the cache

---

_Comment by @konstin on 2024-09-19 11:24_

The cache dir is set unconditionally at https://github.com/astral-sh/setup-uv/blob/ce0062aac78b2f340c6612e0e40b80a2fa668ced/src/setup-uv.ts#L48. We can't unset an env var (https://github.com/actions/runner/issues/1126), but we should also in general make our test suite robust against `UV_CACHE_DIR` being set. Follow-up #7542

---

_Merged by @konstin on 2024-09-19 11:32_

---

_Closed by @konstin on 2024-09-19 11:32_

---

_Branch deleted on 2024-09-19 11:32_

---
