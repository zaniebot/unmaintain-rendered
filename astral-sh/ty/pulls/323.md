```yaml
number: 323
title: "fix(doc): consistent numbering in CONTRIBUTING.md"
type: pull_request
state: closed
author: samypr100
labels: []
assignees: []
base: main
head: doc-fix
created_at: 2025-05-12T02:26:36Z
updated_at: 2025-05-12T11:41:38Z
url: https://github.com/astral-sh/ty/pull/323
synced_at: 2026-01-10T02:34:10Z
```

# fix(doc): consistent numbering in CONTRIBUTING.md

---

_Pull request opened by @samypr100 on 2025-05-12 02:26_

## Summary

While reviewing contributing.md, I noticed numbering was a tad off although GH still renders this correctly.


---

_Comment by @MichaReiser on 2025-05-12 06:31_

This is intentional and [supported by markdown](https://spec.commonmark.org/0.30/#start-number)

Using `1.` for all items has the advantage that it doesn't require changing all numbers when adding a new step (which is very easy to miss)

---

_Closed by @MichaReiser on 2025-05-12 06:31_

---

_Branch deleted on 2025-05-12 11:41_

---
