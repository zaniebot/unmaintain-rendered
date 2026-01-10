```yaml
number: 15117
title: "chore(ci): address linting findings in sync-python-releases.yml"
type: pull_request
state: merged
author: woodruffw
labels:
  - internal
assignees: []
merged: true
base: main
head: ww/more-ci-fixes
created_at: 2025-08-06T19:51:03Z
updated_at: 2025-08-06T20:45:21Z
url: https://github.com/astral-sh/uv/pull/15117
synced_at: 2026-01-10T06:44:33Z
```

# chore(ci): address linting findings in sync-python-releases.yml

---

_Pull request opened by @woodruffw on 2025-08-06 19:51_

## Summary

Continuing to burn these down, one at a time.

This eliminates some implicit credentials, moves a permission block to its minimum scope of effect, and removes an (unexploitable) template expansion.

@konstin to answer your earlier question: I tried `permissions:` this time and got a syntax warning, so I suspect it _needs_ to be an empty mapping object here 🙂 

## Test Plan

I will manually dispatch this workflow once the PR is open.

Edit: Dispatched: https://github.com/astral-sh/uv/actions/runs/16787049700/job/47540074086

---

_Review requested from @charliermarsh by @woodruffw on 2025-08-06 19:51_

---

_Review requested from @konstin by @woodruffw on 2025-08-06 19:51_

---

_Assigned to @woodruffw by @woodruffw on 2025-08-06 19:51_

---

_Label `internal` added by @woodruffw on 2025-08-06 19:51_

---

_@zanieb approved on 2025-08-06 20:45_

---

_Merged by @zanieb on 2025-08-06 20:45_

---

_Closed by @zanieb on 2025-08-06 20:45_

---

_Branch deleted on 2025-08-06 20:45_

---
