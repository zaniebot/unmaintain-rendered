```yaml
number: 4840
title: Add progress bar when downloading python
type: pull_request
state: merged
author: j178
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: download-progress
created_at: 2024-07-05T22:06:15Z
updated_at: 2024-07-07T20:01:35Z
url: https://github.com/astral-sh/uv/pull/4840
synced_at: 2026-01-10T13:48:28Z
```

# Add progress bar when downloading python

---

_Pull request opened by @j178 on 2024-07-05 22:06_

## Summary

Resolves #4825 

## Test Plan

```sh
$ cargo run -- python install --force --preview
$ cargo run -- venv -p 3.12 --python-preference only-managed
$ cargo run -- tool install --preview -p 3.12 --python-preference only-managed --force black
````


---

_Review comment by @charliermarsh on `crates/uv-python/src/downloads.rs`:450 on 2024-07-05 22:07_

I was kinda hoping to avoid hashing when `self.sha256` is `None`, any chance we can preserve that behavior?

---

_@charliermarsh reviewed on 2024-07-05 22:07_

---

_Comment by @charliermarsh on 2024-07-05 22:08_

Thanks for doing this, awesome!

---

_Review comment by @j178 on `crates/uv-python/src/downloads.rs`:450 on 2024-07-05 22:45_

If `self.sha256` is `None`, `hashers` will be empty, and `HashReader` won't do any hashing. So I think the overhead might be negligible. 
https://github.com/astral-sh/uv/blob/1bd73a7346ca62e3a7440f32980fa59591535610/crates/uv-extract/src/hash.rs#L138-L140

---

_@j178 reviewed on 2024-07-05 22:45_

---

_Marked ready for review by @j178 on 2024-07-06 02:04_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-06 19:20_

---

_Label `cli` added by @charliermarsh on 2024-07-06 19:20_

---

_Label `preview` added by @charliermarsh on 2024-07-06 19:20_

---

_Comment by @charliermarsh on 2024-07-06 19:23_

Can we try to omit the `░░░░░░░░░░░░░░░░ [1/1] Downloading Python...` when we're downloading exactly one version?

---

_@charliermarsh approved on 2024-07-07 19:51_

---

_Merged by @charliermarsh on 2024-07-07 20:01_

---

_Closed by @charliermarsh on 2024-07-07 20:01_

---
