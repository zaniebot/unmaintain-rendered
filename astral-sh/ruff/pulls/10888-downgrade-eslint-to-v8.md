```yaml
number: 10888
title: Downgrade ESLint to v8
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/es
created_at: 2024-04-11T17:17:11Z
updated_at: 2024-04-11T17:23:47Z
url: https://github.com/astral-sh/ruff/pull/10888
synced_at: 2026-01-10T22:37:01Z
```

# Downgrade ESLint to v8

---

_Pull request opened by @charliermarsh on 2024-04-11 17:17_

## Summary

Some of our plugins aren't compatible with v9.

Originally shipped in #10827.

## Test Plan

- `npm install`
- `npm ci`


---

_Review requested from @dhruvmanila by @charliermarsh on 2024-04-11 17:17_

---

_Label `internal` added by @charliermarsh on 2024-04-11 17:17_

---

_@dhruvmanila approved on 2024-04-11 17:19_

Thank you!

---

_Merged by @dhruvmanila on 2024-04-11 17:23_

---

_Closed by @dhruvmanila on 2024-04-11 17:23_

---

_Branch deleted on 2024-04-11 17:23_

---
