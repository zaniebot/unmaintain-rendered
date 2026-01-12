```yaml
number: 17317
title: Filter PPC wheels et al in lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/fil
created_at: 2026-01-04T19:01:43Z
updated_at: 2026-01-05T17:07:00Z
url: https://github.com/astral-sh/uv/pull/17317
synced_at: 2026-01-12T16:12:43Z
```

# Filter PPC wheels et al in lockfile

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/17313.


---

_Label `bug` added by @charliermarsh on 2026-01-04 19:01_

---

_Marked ready for review by @charliermarsh on 2026-01-04 19:01_

---

_Merged by @charliermarsh on 2026-01-05 15:25_

---

_Closed by @charliermarsh on 2026-01-05 15:25_

---

_Branch deleted on 2026-01-05 15:25_

---

_@konstin reviewed on 2026-01-05 17:07_

---

_Review comment by @konstin on `crates/uv-platform-tags/src/platform_tag.rs`:385 on 2026-01-05 17:07_

is it intentional that we're only matching against the current manylinux support instead of something more generic like `self.arch().is_some_and(|arch| arch == Arch::Armv6L)`?

---
