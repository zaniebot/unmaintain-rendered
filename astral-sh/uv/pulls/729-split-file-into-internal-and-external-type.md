```yaml
number: 729
title: "Split `File` into internal and external type"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/split-file
created_at: 2023-12-25T20:04:19Z
updated_at: 2023-12-25T20:42:29Z
url: https://github.com/astral-sh/uv/pull/729
synced_at: 2026-01-10T15:44:44Z
```

# Split `File` into internal and external type

---

_Pull request opened by @charliermarsh on 2023-12-25 20:04_

## Summary

This PR makes the `pypi_types::File` a response-only type (i.e., a type that's only used when deserializing over the wire), and adds a separate internal `File` type. Right now, the representations are similar, but already, we can avoid the "lenient" deserialization on our internal `File` type, and avoid the special-casing of the property names that's required in the JSON. Over time, we can evolve this representation entirely separately from the representation we receive from PyPI and other indexes.


---

_Merged by @charliermarsh on 2023-12-25 20:42_

---

_Closed by @charliermarsh on 2023-12-25 20:42_

---

_Branch deleted on 2023-12-25 20:42_

---
