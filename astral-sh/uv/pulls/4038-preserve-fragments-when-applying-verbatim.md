```yaml
number: 4038
title: Preserve fragments when applying verbatim redirects
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/frag
created_at: 2024-06-05T03:43:58Z
updated_at: 2024-06-05T03:53:24Z
url: https://github.com/astral-sh/uv/pull/4038
synced_at: 2026-01-12T16:06:00Z
```

# Preserve fragments when applying verbatim redirects

---

_@charliermarsh_

## Summary

`echo "git+https://github.com/pypa/sample-namespace-packages.git#subdirectory=pkg_resources/pkg_a" | cargo run pip compile -` now resolves to a compliant URL.

Closes https://github.com/astral-sh/uv/issues/4037.


---

_Marked ready for review by @charliermarsh on 2024-06-05 03:44_

---

_Label `bug` added by @charliermarsh on 2024-06-05 03:44_

---

_@charliermarsh reviewed on 2024-06-05 03:46_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:1568 on 2024-06-05 03:46_

So we actually had a case exercising this, hah, but the expectation was wrong.

---

_Merged by @charliermarsh on 2024-06-05 03:53_

---

_Closed by @charliermarsh on 2024-06-05 03:53_

---

_Branch deleted on 2024-06-05 03:53_

---
