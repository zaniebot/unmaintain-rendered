```yaml
number: 6718
title: Update project documentation for the application / library concepts
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/project-docs-app
created_at: 2024-08-27T19:41:05Z
updated_at: 2024-08-27T21:22:39Z
url: https://github.com/astral-sh/uv/pull/6718
synced_at: 2026-01-12T16:07:30Z
```

# Update project documentation for the application / library concepts

---

_@zanieb_

Follows https://github.com/astral-sh/uv/pull/6689 and https://github.com/astral-sh/uv/pull/6585

---

_Renamed from "zb/project docs app" to "Update project documentation for the application / library concepts" by @zanieb on 2024-08-27 19:41_

---

_@charliermarsh reviewed on 2024-08-27 19:43_

---

_Review comment by @charliermarsh on `docs/concepts/projects.md`:57 on 2024-08-27 19:43_

two "primary" maybe? Or just "two kinds of projects"?

---

_@charliermarsh reviewed on 2024-08-27 19:43_

---

_Review comment by @charliermarsh on `docs/concepts/projects.md`:61 on 2024-08-27 19:43_

for for

---

_@charliermarsh reviewed on 2024-08-27 19:43_

---

_Review comment by @charliermarsh on `docs/concepts/projects.md`:65 on 2024-08-27 19:43_

End these in a period IMO

---

_@charliermarsh reviewed on 2024-08-27 19:44_

---

_Review comment by @charliermarsh on `docs/concepts/projects.md`:83 on 2024-08-27 19:44_

for example, uploaded to PyPI

---

_@charliermarsh reviewed on 2024-08-27 19:44_

---

_Review comment by @charliermarsh on `docs/concepts/projects.md`:89 on 2024-08-27 19:44_

in a `src/{package}/...` directory

---

_Review comment by @charliermarsh on `docs/concepts/projects.md`:90 on 2024-08-27 19:44_

Should we say "virtual environment" here?

---

_@charliermarsh reviewed on 2024-08-27 19:44_

---

_@charliermarsh reviewed on 2024-08-27 19:45_

---

_Review comment by @charliermarsh on `docs/guides/publish.md`:20 on 2024-08-27 19:45_

to distributable packages?

---

_@charliermarsh reviewed on 2024-08-27 19:45_

---

_Review comment by @charliermarsh on `docs/guides/publish.md`:26 on 2024-08-27 19:45_

Just from reading this, what is the difference between `--lib` and `--package`?

---

_@zanieb reviewed on 2024-08-27 20:15_

---

_Review comment by @zanieb on `docs/concepts/projects.md`:65 on 2024-08-27 20:15_

Per my own style guide 😭 

---

_Review comment by @zanieb on `docs/concepts/projects.md`:90 on 2024-08-27 20:16_

I generally try to talk about the "project environment" or, if ambiguous, "project virtual environment". Just to move the virtual environment concept further from the users.

---

_@zanieb reviewed on 2024-08-27 20:16_

---

_@charliermarsh reviewed on 2024-08-27 20:48_

---

_Review comment by @charliermarsh on `docs/concepts/projects.md`:65 on 2024-08-27 20:48_

But this isn't _required_ right?

---

_@charliermarsh approved on 2024-08-27 20:48_

---

_@zanieb reviewed on 2024-08-27 21:09_

---

_Review comment by @zanieb on `docs/concepts/projects.md`:65 on 2024-08-27 21:09_

Well.. I wrote it that way at first but that gets confusing. I think we have

- Applications: no build backend
- Libraries: yes build backend
- Other: you set `package` manually

---

_Marked ready for review by @zanieb on 2024-08-27 21:22_

---

_Merged by @zanieb on 2024-08-27 21:22_

---

_Closed by @zanieb on 2024-08-27 21:22_

---

_Branch deleted on 2024-08-27 21:22_

---

_Label `documentation` added by @zanieb on 2024-08-27 21:22_

---
