```yaml
number: 1843
title: Treat ARM wheels as higher-priority than universal
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/priority
created_at: 2024-02-22T01:10:58Z
updated_at: 2024-02-22T01:38:57Z
url: https://github.com/astral-sh/uv/pull/1843
synced_at: 2026-01-10T14:54:43Z
```

# Treat ARM wheels as higher-priority than universal

---

_Pull request opened by @charliermarsh on 2024-02-22 01:10_

## Summary

We need to take care to keep wheel tags in "priority order" (e.g., we should prefer ARM wheels over universal wheels). However... it looks like we've had a `.sort()` in here all along, that risks throwing off the ordering?

Closes https://github.com/astral-sh/uv/issues/1840.

## Test Plan

ensure that `rlax` uses the ARM wheel rather than the universal wheel:

- `cargo run venv`
- `cargo run pip install rlax`
- `import rlax`


---

_Marked ready for review by @charliermarsh on 2024-02-22 01:11_

---

_Review comment by @charliermarsh on `crates/platform-tags/src/lib.rs`:258 on 2024-02-22 01:12_

(Not sure if there's a better way to do this.)

---

_@charliermarsh reviewed on 2024-02-22 01:12_

---

_Comment by @charliermarsh on 2024-02-22 01:12_

This will probably fix a variety of ARM failures.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-02-22 01:14_

---

_Label `bug` added by @charliermarsh on 2024-02-22 01:14_

---

_Comment by @charliermarsh on 2024-02-22 01:24_

@BurntSushi caught that this is the wrong fix -- there's a `.reverse()` I missed at the top, the logic is right.

What's wrong is the priority ordering of some tags.

---

_Converted to draft by @charliermarsh on 2024-02-22 01:24_

---

_@BurntSushi approved on 2024-02-22 01:36_

---

_Marked ready for review by @charliermarsh on 2024-02-22 01:36_

---

_Merged by @charliermarsh on 2024-02-22 01:38_

---

_Closed by @charliermarsh on 2024-02-22 01:38_

---

_Branch deleted on 2024-02-22 01:38_

---
