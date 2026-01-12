```yaml
number: 294
title: "docs: Document configuration file discovery"
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: main
head: micha/configuration-files
created_at: 2025-05-09T13:57:51Z
updated_at: 2025-05-10T10:35:27Z
url: https://github.com/astral-sh/ty/pull/294
synced_at: 2026-01-12T15:54:27Z
```

# docs: Document configuration file discovery

---

_@MichaReiser_

## Summary

This is a 95% copy from the uv docs with the main difference that I replaced uv with ty ;) 

There are some small differences:

* ty doesn't support system level configurations (yet?)
* ty doesn't support any configuration environment variables (yet)
* ty's array merging is different from uv's



---

_Label `documentation` added by @MichaReiser on 2025-05-09 13:57_

---

_Review comment by @carljm on `README.md`:108 on 2025-05-09 13:59_

```suggestion
If project- and user-level configuration files are found, the settings will be merged, with project-level configuration taking precedence over the user-level configuration.
```

---

_@carljm approved on 2025-05-09 14:00_

Thank you!

---

_@zanieb reviewed on 2025-05-09 14:14_

---

_Review comment by @zanieb on `README.md`:114 on 2025-05-09 14:14_

This doesn't exist, does it?

---

_@zanieb approved on 2025-05-09 14:14_

---

_@zanieb reviewed on 2025-05-09 14:14_

---

_Review comment by @zanieb on `README.md`:114 on 2025-05-09 14:14_

Nevermind, different base branch

---

_Merged by @MichaReiser on 2025-05-10 10:35_

---

_Closed by @MichaReiser on 2025-05-10 10:35_

---

_Branch deleted on 2025-05-10 10:35_

---
