```yaml
number: 327
title: "Add \"rules\" and \"checking a project\" sections"
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: main
head: micha/more-docs
created_at: 2025-05-12T12:39:44Z
updated_at: 2025-05-12T16:52:26Z
url: https://github.com/astral-sh/ty/pull/327
synced_at: 2026-01-10T02:34:10Z
```

# Add "rules" and "checking a project" sections

---

_Pull request opened by @MichaReiser on 2025-05-12 12:39_

## Summary

Add documentation about what rules are and how to type check a project



---

_Label `documentation` added by @MichaReiser on 2025-05-12 12:39_

---

_@MichaReiser reviewed on 2025-05-12 12:40_

---

_Review comment by @MichaReiser on `README.md`:370 on 2025-05-12 12:40_

I switched to using full URls because pypi doesn't support relative links

---

_@zanieb reviewed on 2025-05-12 14:35_

---

_Review comment by @zanieb on `README.md`:370 on 2025-05-12 14:35_

We should probably restore the PyPI readme transform script so these links work properly on branches in PRs, but this seems fine to start

---

_@MichaReiser reviewed on 2025-05-12 14:48_

---

_Review comment by @MichaReiser on `README.md`:370 on 2025-05-12 14:48_

Doesn't it just replace badges/images? But yes, this is something we could add to it as well

---

_Renamed from "Document *rules* and *checking a project*" to "Add "rules" and "checking a project" sections" by @MichaReiser on 2025-05-12 14:49_

---

_@zanieb reviewed on 2025-05-12 14:54_

---

_Review comment by @zanieb on `README.md`:370 on 2025-05-12 14:54_

It looks like in uv the relative link is turned into a tagged link? (from looking at PyPI)

---

_@MichaReiser reviewed on 2025-05-12 14:56_

---

_Review comment by @MichaReiser on `README.md`:370 on 2025-05-12 14:56_

that's cool!

---

_Marked ready for review by @zanieb on 2025-05-12 16:52_

---

_Merged by @zanieb on 2025-05-12 16:52_

---

_Closed by @zanieb on 2025-05-12 16:52_

---

_Branch deleted on 2025-05-12 16:52_

---
