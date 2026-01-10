```yaml
number: 7537
title: Clarify behavior of of overrides in CLI reference
type: pull_request
state: merged
author: YouJiacheng
labels:
  - documentation
assignees: []
merged: true
base: main
head: override-dependencies-docs
created_at: 2024-09-19T08:32:19Z
updated_at: 2024-09-19T12:02:42Z
url: https://github.com/astral-sh/uv/pull/7537
synced_at: 2026-01-10T12:53:49Z
```

# Clarify behavior of of overrides in CLI reference

---

_Pull request opened by @YouJiacheng on 2024-09-19 08:32_

## Summary
Improve the description of override-dependencies based on the statement in `concepts/resolution.md`: "As with constraints, overrides do not add a dependency on the package and only take effect if the package is requested in a direct or transitive dependency."

I tested it locally, `concepts/resolution.md` is correct. It would be better to also include this in the Reference Chapter of the docs.




---

_@zanieb approved on 2024-09-19 12:01_

Thanks!

---

_Renamed from "Improve docs: `settings/override-dependencies`" to "Clarify behavior of of overrides in CLI reference" by @zanieb on 2024-09-19 12:02_

---

_Label `documentation` added by @zanieb on 2024-09-19 12:02_

---

_Merged by @zanieb on 2024-09-19 12:02_

---

_Closed by @zanieb on 2024-09-19 12:02_

---
