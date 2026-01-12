```yaml
number: 8996
title: Add openSUSE Tumbleweed into install doc
type: pull_request
state: merged
author: mimi1vx
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2023-12-04T13:00:48Z
updated_at: 2023-12-06T17:08:00Z
url: https://github.com/astral-sh/ruff/pull/8996
synced_at: 2026-01-10T23:40:55Z
```

# Add openSUSE Tumbleweed into install doc

---

_Pull request opened by @mimi1vx on 2023-12-04 13:00_

_No description provided._

---

_@zanieb reviewed on 2023-12-04 15:08_

Thanks for contributing to the project :)

I'm a little worried this is too specific, I don't think we should necessarily enumerate every Linux distribution. Curious to hear what others think though.

---

_Review comment by @MichaReiser on `docs/installation.md`:59 on 2023-12-05 07:54_

Nit: Maybe move this above the Docker documentation so that all linux distribution specific installation methods are together.

---

_Comment by @MichaReiser on 2023-12-05 07:55_

> I'm a little worried this is too specific, I don't think we should necessarily enumerate every Linux distribution. Curious to hear what others think though.

We already list other Linux distributions like Arch and Alpine linux. Is your concern about us doing this in general or is it a new concern because the list becomes longer?

---

_Label `documentation` added by @MichaReiser on 2023-12-05 07:55_

---

_Comment by @mimi1vx on 2023-12-05 08:51_

plus for openSUSE there is difference because is packaged as standard python package --> so name is python3-ruff

---

_@MichaReiser reviewed on 2023-12-05 08:53_

---

_Comment by @konstin on 2023-12-05 08:59_

If the linux list becomes too long we could switch to a markdown table for all linux distros

---

_Comment by @zanieb on 2023-12-06 15:57_

I'm concerned about the list becoming longer. I feel like Arch and Alpine are special cases, maybe Ubuntu as well. I'm curious if there's an example of a project that lists a bunch of distributions nicely?

Regardless, we can merge this then remove later if it becomes a problem.

---

_Comment by @charliermarsh on 2023-12-06 17:05_

Sounds good. We can merge for now and revisit if it becomes a problem...

---

_Merged by @charliermarsh on 2023-12-06 17:08_

---

_Closed by @charliermarsh on 2023-12-06 17:08_

---
