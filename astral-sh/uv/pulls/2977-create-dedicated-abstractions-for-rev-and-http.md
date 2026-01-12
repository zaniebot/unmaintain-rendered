```yaml
number: 2977
title: "Create dedicated abstractions for `.rev` and `.http` pointers"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/rev
created_at: 2024-04-10T20:44:56Z
updated_at: 2024-04-10T21:30:28Z
url: https://github.com/astral-sh/uv/pull/2977
synced_at: 2026-01-12T16:05:21Z
```

# Create dedicated abstractions for `.rev` and `.http` pointers

---

_@charliermarsh_

## Summary

This PR formalizes some of the concepts we use in the cache for "pointers to things".

In the wheel cache, we have files like `annotated_types-0.6.0-py3-none-any.http`. This represents an unzipped wheel, cached alongside an HTTP caching policy. We now have a struct for this to encapsulate the logic: `HttpArchivePointer`.

Similarly, we have files like `annotated_types-0.6.0-py3-none-any.rev`. This represents an unzipped local wheel, alongside with a timestamp. We now have a struct for this to encapsulate the logic: `LocalArchivePointer`.

We have similar structs for source distributions too.


---

_Marked ready for review by @charliermarsh on 2024-04-10 20:46_

---

_Label `internal` added by @charliermarsh on 2024-04-10 20:48_

---

_Renamed from "Clean up `.rev` and `.http` reads" to "Create dedicated abstractions for `.rev` and `.http` pointers" by @charliermarsh on 2024-04-10 21:25_

---

_Merged by @charliermarsh on 2024-04-10 21:30_

---

_Closed by @charliermarsh on 2024-04-10 21:30_

---

_Branch deleted on 2024-04-10 21:30_

---
