```yaml
number: 7623
title: Remove double whitespaces from the code
type: pull_request
state: merged
author: Stanley5249
labels:
  - documentation
assignees: []
merged: true
base: main
head: remove-spaces
created_at: 2024-09-22T16:44:59Z
updated_at: 2024-09-24T02:41:55Z
url: https://github.com/astral-sh/uv/pull/7623
synced_at: 2026-01-12T16:07:55Z
```

# Remove double whitespaces from the code

---

_@Stanley5249_

## Summary

I noticed that when running `uv help build`, the help messages had redundant spaces. I searched for and replaced these redundant spaces and fixed some other similar cases.

This is my first commit to this repository, and I'm not very familiar with how comments are made into CLI messages, but I believe these changes should work.

---

_Comment by @Stanley5249 on 2024-09-22 17:00_

It seems something is preventing the changes from affecting the CLI messages. Can someone help?

---

_Comment by @konstin on 2024-09-23 07:02_

We generate schema and docs files from the doc-comments, you can update them with `cargo dev generate-all`

---

_Label `documentation` added by @konstin on 2024-09-23 07:02_

---

_@konstin approved on 2024-09-23 20:09_

Thank you!

---

_Merged by @konstin on 2024-09-23 20:15_

---

_Closed by @konstin on 2024-09-23 20:15_

---

_Branch deleted on 2024-09-24 02:41_

---
