```yaml
number: 11895
title: "Docs: Clarify that setting cache-keys overrides defaults"
type: pull_request
state: merged
author: alexjball
labels:
  - documentation
assignees: []
merged: true
base: main
head: caching-docs-clarification
created_at: 2025-03-02T17:09:07Z
updated_at: 2025-03-03T02:54:49Z
url: https://github.com/astral-sh/uv/pull/11895
synced_at: 2026-01-12T16:10:02Z
```

# Docs: Clarify that setting cache-keys overrides defaults

---

_@alexjball_

## Summary

The current wording on the [caching page](https://docs.astral.sh/uv/concepts/cache/#dynamic-metadata) makes it sounds like defining `cache-keys` in a project adds to the metadata considered when caching. However it actually replaces the metadata. So copying the example using the git commit results in only considering the git commit, not the pyproject.toml, which is likely not what is typically desired.


---

_@charliermarsh approved on 2025-03-03 02:36_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2025-03-03 02:36_

---

_Merged by @charliermarsh on 2025-03-03 02:54_

---

_Closed by @charliermarsh on 2025-03-03 02:54_

---
