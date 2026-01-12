```yaml
number: 12289
title: "add `default-groups = \"all\"` sugar to `pyproject.toml`"
type: pull_request
state: merged
author: Gankra
labels:
  - enhancement
  - configuration
assignees: []
merged: true
base: main
head: gankra/all-groups
created_at: 2025-03-18T16:24:54Z
updated_at: 2025-03-19T09:18:48Z
url: https://github.com/astral-sh/uv/pull/12289
synced_at: 2026-01-12T16:10:12Z
```

# add `default-groups = "all"` sugar to `pyproject.toml`

---

_@Gankra_

Suggested by @zanieb in #10934

* [x] agree we want to do this
* [x] add docs

---

_Comment by @zanieb on 2025-03-18 17:10_

I'm in favor. Thoughts @charliermarsh / @konstin ?

---

_Comment by @johnthagen on 2025-03-18 17:22_

FWIW, as a user looking to migrate from Poetry that has similar functionality in this area, this would be great 🚀.

---

_Comment by @charliermarsh on 2025-03-18 17:28_

Same, I like it.

---

_@johnthagen reviewed on 2025-03-18 17:31_

---

_Review comment by @johnthagen on `docs/reference/settings.md`:115 on 2025-03-18 17:31_

```suggestion
Can also be the literal "all" to default enable all groups.
```

---

_Comment by @Gankra on 2025-03-18 17:36_

Cool, I'll clean this up!

---

_Marked ready for review by @Gankra on 2025-03-18 17:58_

---

_Label `enhancement` added by @Gankra on 2025-03-18 18:07_

---

_Label `configuration` added by @Gankra on 2025-03-18 18:07_

---

_@zanieb reviewed on 2025-03-18 18:08_

---

_Review comment by @zanieb on `docs/concepts/projects/dependencies.md`:687 on 2025-03-18 18:08_

I would just say

```suggestion
To enable all dependencies groups by default, use `"all"` instead of listing group names:
```

---

_@zanieb reviewed on 2025-03-18 18:09_

---

_Review comment by @zanieb on `docs/concepts/projects/dependencies.md`:687 on 2025-03-18 18:09_

We tend to avoid "For projects that..." — we're talking to the user here.


---

_@zanieb approved on 2025-03-18 18:10_

---

_Merged by @zanieb on 2025-03-18 18:42_

---

_Closed by @zanieb on 2025-03-18 18:42_

---

_Branch deleted on 2025-03-18 18:42_

---

_@konstin reviewed on 2025-03-19 09:18_

I like it!

---
