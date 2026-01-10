```yaml
number: 1182
title: Accept relative file paths in CLI requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/p
created_at: 2024-01-30T03:09:27Z
updated_at: 2024-01-30T03:31:25Z
url: https://github.com/astral-sh/uv/pull/1182
synced_at: 2026-01-10T15:39:03Z
```

# Accept relative file paths in CLI requirements

---

_Pull request opened by @charliermarsh on 2024-01-30 03:09_

## Summary

See: https://github.com/astral-sh/puffin/issues/1181.

## Test Plan

```
❯ cargo run -- pip install packse@../../zanieb/packse
    Finished dev [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/puffin pip install 'packse@../../zanieb/packse'`
error: Distribution not found at: file:///Users/crmarsh/zanieb/packse
```


---

_Review requested from @zanieb by @charliermarsh on 2024-01-30 03:09_

---

_Label `bug` added by @charliermarsh on 2024-01-30 03:09_

---

_@charliermarsh reviewed on 2024-01-30 03:09_

---

_Review comment by @charliermarsh on `crates/puffin/src/requirements.rs`:88 on 2024-01-30 03:09_

This is the exact fix: `from_str` calls `parse(name, None)` internally, which doesn't allow relative!

---

_Comment by @zanieb on 2024-01-30 03:16_

Great! It failed because it won't download a `git` dependency which is annoying but otherwise lgtm

---

_@zanieb approved on 2024-01-30 03:16_

---

_Merged by @charliermarsh on 2024-01-30 03:31_

---

_Closed by @charliermarsh on 2024-01-30 03:31_

---

_Branch deleted on 2024-01-30 03:31_

---
