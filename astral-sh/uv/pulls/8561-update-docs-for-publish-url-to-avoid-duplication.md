```yaml
number: 8561
title: "Update docs for `--publish-url` to avoid duplication."
type: pull_request
state: merged
author: nathanjmcdougall
labels: []
assignees: []
merged: true
base: main
head: docs/publish-url-sentence-dupe
created_at: 2024-10-25T10:08:38Z
updated_at: 2024-10-25T13:55:33Z
url: https://github.com/astral-sh/uv/pull/8561
synced_at: 2026-01-10T12:54:12Z
```

# Update docs for `--publish-url` to avoid duplication.

---

_Pull request opened by @nathanjmcdougall on 2024-10-25 10:08_

## Summary

These two sentences in the docs for `--publish-url` seem to basically be duplicates:

https://github.com/astral-sh/uv/blob/3eda248ef5678fe07b5424fb4f256011800fbb15/crates/uv-cli/src/lib.rs#L4616-L4618

I found the first to be easier to read, so this commit removes the second.

## Test Plan

No tests, change is docs-only.


---

_@zanieb approved on 2024-10-25 13:32_

Thanks!

---

_Merged by @zanieb on 2024-10-25 13:55_

---

_Closed by @zanieb on 2024-10-25 13:55_

---
