```yaml
number: 5883
title: "feat: add arg index to error context"
type: pull_request
state: closed
author: jordanjennings-mysten
labels: []
assignees: []
draft: true
base: master
head: add-arg-index-to-error-context
created_at: 2025-01-17T00:52:22Z
updated_at: 2025-03-10T02:35:17Z
url: https://github.com/clap-rs/clap/pull/5883
synced_at: 2026-01-12T16:14:17Z
```

# feat: add arg index to error context

---

_@jordanjennings-mysten_

Adds arg index to the error context, LMK if this seems like a reasonable approach. We would like to utilize this information to create "at line/character XYZ" style errors using miette.

related issue
#4948 

---
