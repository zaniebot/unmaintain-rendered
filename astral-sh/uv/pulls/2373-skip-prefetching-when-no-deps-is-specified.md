```yaml
number: 2373
title: "Skip prefetching when `--no-deps` is specified"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/deps
created_at: 2024-03-12T03:29:57Z
updated_at: 2024-03-12T03:44:03Z
url: https://github.com/astral-sh/uv/pull/2373
synced_at: 2026-01-12T16:05:00Z
```

# Skip prefetching when `--no-deps` is specified

---

_@charliermarsh_

## Summary

When running under `--no-deps`, we don't need to pre-fetch, because pre-fetching fetches the _distribution_ metadata. But with `--no-deps`, we only need the package metadata for the top-level requirements. We never need distribution metadata.

Incidentally, this will fix https://github.com/astral-sh/uv/issues/2300.

## Test Plan

- `cargo test`
- `./target/debug/uv  pip install --verbose --no-cache-dir --no-deps --reinstall ddtrace==2.6.2 debugpy==1.8.1 ecdsa==0.18.0 editorconfig==0.12.4 --verbose` in a Python 3.10 Docker contain repeatedly.


---

_Label `bug` added by @charliermarsh on 2024-03-12 03:30_

---

_Marked ready for review by @charliermarsh on 2024-03-12 03:30_

---

_Comment by @charliermarsh on 2024-03-12 03:30_

There is a more complete fix for #2300 that I will put up separately, but this does fix the proximate cause.

---

_Merged by @charliermarsh on 2024-03-12 03:44_

---

_Closed by @charliermarsh on 2024-03-12 03:44_

---

_Branch deleted on 2024-03-12 03:44_

---
