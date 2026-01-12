```yaml
number: 14295
title: "Add a \"choosing a build backend\" section to the docs"
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/build-backend-docs-hatchling
created_at: 2025-06-26T20:40:28Z
updated_at: 2025-07-02T14:02:04Z
url: https://github.com/astral-sh/uv/pull/14295
synced_at: 2026-01-12T16:11:08Z
```

# Add a "choosing a build backend" section to the docs

---

_@konstin_

I think the build backend docs as a whole are now ready for review. I only made a small change here.

---

_Review requested from @zanieb by @konstin on 2025-06-26 20:40_

---

_Label `preview` added by @konstin on 2025-06-26 20:40_

---

_@konstin reviewed on 2025-06-26 20:48_

---

_Review comment by @konstin on `docs/concepts/build-backend.md`:15 on 2025-06-26 20:48_

I wonder if we should be clearer about the value proposition here. The uv build backend ...

* comes with good defaults, you don't need to configure packages or anything to get started. This makes it simple, and a great choice to start with.
* is misuse resistant. We are strict with the metadata, we check for the `__init__.py`, we don't allow bad include structures, we don't consider git configuration. This makes having a wheel linter unnecessary.
* is configurable. All you should need for a pure Python project layout should be possible.
* is integrated, for better warnings, errors and general outputs
* is fast (not that others are notably slow with uv)

---

_@zanieb reviewed on 2025-06-27 17:37_

---

_Review comment by @zanieb on `docs/concepts/build-backend.md`:15 on 2025-06-27 17:37_

I'm supportive of having a list like that, it seems good!

---

_@zanieb reviewed on 2025-06-27 17:38_

---

_Review comment by @zanieb on `docs/concepts/build-backend.md`:26 on 2025-06-27 17:38_

```suggestion
While it supports a number of options for configuring your project structure, if build scripts or
a more flexible project layout are required, consider using the
[hatchling](https://hatch.pypa.io/latest/config/build/#build-system) build backend instead.
```

---

_@zanieb approved on 2025-06-27 17:38_

I'll take a minute to read the whole page and post a pull request with any edits.

---

_Renamed from "Build backend documentation" to "Add a "choosing a build backend" section to the docs" by @zanieb on 2025-07-02 14:01_

---

_Merged by @zanieb on 2025-07-02 14:02_

---

_Closed by @zanieb on 2025-07-02 14:02_

---

_Branch deleted on 2025-07-02 14:02_

---
