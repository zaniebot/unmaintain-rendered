```yaml
number: 6519
title: Avoid JSON parse error on playground load
type: pull_request
state: merged
author: charliermarsh
labels:
  - playground
assignees: []
merged: true
base: main
head: charlie/error
created_at: 2023-08-12T04:02:20Z
updated_at: 2023-08-12T04:11:45Z
url: https://github.com/astral-sh/ruff/pull/6519
synced_at: 2026-01-12T15:55:21Z
```

# Avoid JSON parse error on playground load

---

_@charliermarsh_

## Summary

On page load, the playground very briefly flickers a JSON parse error. Due to our use of `useDeferredValue`, we attempt to parse the empty JSON string settings, since after `const initialized = ruffVersion != null;` returns true, we get one render with the stale deferred value.

This PR refactors the state, such that we start by storing `null` for the `Source`, and use the `Source` itself to determine initialization status.

## Test Plan

Set a breakpoint in the `catch` path in `Editor`; verified that it no longer triggers on load (but did on `main`).


---

_Label `playground` added by @charliermarsh on 2023-08-12 04:02_

---

_Merged by @charliermarsh on 2023-08-12 04:11_

---

_Closed by @charliermarsh on 2023-08-12 04:11_

---

_Branch deleted on 2023-08-12 04:11_

---
