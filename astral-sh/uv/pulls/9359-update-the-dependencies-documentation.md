```yaml
number: 9359
title: Update the dependencies documentation
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/dependencies
created_at: 2024-11-22T17:45:16Z
updated_at: 2024-11-26T18:32:59Z
url: https://github.com/astral-sh/uv/pull/9359
synced_at: 2026-01-10T12:00:00Z
```

# Update the dependencies documentation

---

_Pull request opened by @zanieb on 2024-11-22 17:45_

This is a first-pass at updating the "Managing dependencies" page after moving some of the project concept documentation into it. I want to do more things, like improve visibility into upgrading packages and reordering some sections, but will tackle those separately for review.

The primary goals here were to consolidate redundant information on dependency tables and improve the consistency of examples.

---

_Label `documentation` added by @zanieb on 2024-11-22 17:45_

---

_Marked ready for review by @zanieb on 2024-11-22 17:47_

---

_Review comment by @konstin on `docs/concepts/projects/dependencies.md`:219 on 2024-11-26 16:15_

We should talk about when you need `tool.uv.sources` and that we're doing this due to holes in the spec (no relative paths, no workspace support)

---

_@konstin approved on 2024-11-26 16:19_

---

_@zanieb reviewed on 2024-11-26 17:03_

---

_Review comment by @zanieb on `docs/concepts/projects/dependencies.md`:219 on 2024-11-26 17:03_

That's included above

> Dependency sources add support common patterns that are not supported by the `project.dependencies` standard, like editable installations and relative paths. For example, to install `foo` from a directory relative to the project root:

---

_Merged by @zanieb on 2024-11-26 18:32_

---

_Closed by @zanieb on 2024-11-26 18:32_

---

_Branch deleted on 2024-11-26 18:32_

---
