```yaml
number: 4468
title: Respect index strategy in source distribution builds
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/i
created_at: 2024-06-24T11:53:57Z
updated_at: 2024-06-24T12:03:39Z
url: https://github.com/astral-sh/uv/pull/4468
synced_at: 2026-01-10T13:48:28Z
```

# Respect index strategy in source distribution builds

---

_Pull request opened by @charliermarsh on 2024-06-24 11:53_

## Summary

The `--index-strategy` is linked to the index locations, which we propagate to source distribution builds; so it makes sense to pass the `--index-strategy` too.

While I was here, I made `exclude_newer` a required argument so that we don't forget to set it via the `with_options` builder.

Closes https://github.com/astral-sh/uv/issues/4465.


---

_Label `bug` added by @charliermarsh on 2024-06-24 11:54_

---

_Merged by @charliermarsh on 2024-06-24 12:03_

---

_Closed by @charliermarsh on 2024-06-24 12:03_

---

_Branch deleted on 2024-06-24 12:03_

---
