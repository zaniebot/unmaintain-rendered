```yaml
number: 1255
title: Search and replace windows specific tests in deps
type: pull_request
state: merged
author: konstin
labels:
  - windows
assignees: []
merged: true
base: main
head: konsti/windows-regex-filters
created_at: 2024-02-05T22:18:29Z
updated_at: 2024-02-06T19:31:43Z
url: https://github.com/astral-sh/uv/pull/1255
synced_at: 2026-01-10T15:33:24Z
```

# Search and replace windows specific tests in deps

---

_Pull request opened by @konstin on 2024-02-05 22:18_

Remove windows-only dependencies from the snapshot output using regex. We now do the filtering entirely on our without relying on insta settings.

435 tests run: 430 passed (30 slow), 5 failed, 1 skipped

---

_Review comment by @konstin on `crates/puffin/tests/common/mod.rs`:207 on 2024-02-05 22:22_

This clearly isn't ideal, it just seems to work for all of our cases. I'm also not happy about special cased programmatic windows filters but otoh we don't have the need for a proper programmatic filter system.

---

_@konstin reviewed on 2024-02-05 22:22_

---

_Label `windows` added by @konstin on 2024-02-05 23:22_

---

_@charliermarsh approved on 2024-02-06 00:44_

Let's see how this plays out :joy:

---

_Merged by @konstin on 2024-02-06 19:31_

---

_Closed by @konstin on 2024-02-06 19:31_

---

_Branch deleted on 2024-02-06 19:31_

---
