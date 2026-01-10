```yaml
number: 7444
title: Avoid erroneous version warning for .dist-info
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - error messages
assignees: []
merged: true
base: main
head: charlie/w
created_at: 2024-09-16T22:22:55Z
updated_at: 2024-09-16T22:32:57Z
url: https://github.com/astral-sh/uv/pull/7444
synced_at: 2026-01-10T12:53:47Z
```

# Avoid erroneous version warning for .dist-info

---

_Pull request opened by @charliermarsh on 2024-09-16 22:22_

## Summary

Since https://github.com/astral-sh/uv/pull/7208, this is now _always_ firing, for every directory, because the version gets normalized (e.g., `1.2.3` gets normalized to `1-2-3`, which never matches the parsed version). pip doesn't warn here, I guess we won't either, because I can't figure out a robust way to do this... We need to get the non-normalized remainder after stripping the normalized package name, but we strip the normalized package name from the normalized string, so we only have a normalized remainder.


---

_Label `bug` added by @charliermarsh on 2024-09-16 22:23_

---

_Label `error messages` added by @charliermarsh on 2024-09-16 22:23_

---

_Merged by @charliermarsh on 2024-09-16 22:32_

---

_Closed by @charliermarsh on 2024-09-16 22:32_

---

_Branch deleted on 2024-09-16 22:32_

---
