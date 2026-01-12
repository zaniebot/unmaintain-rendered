```yaml
number: 5794
title: Add a guide for publishing packages
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: zb/docs-publish
created_at: 2024-08-05T16:54:05Z
updated_at: 2024-08-07T12:48:23Z
url: https://github.com/astral-sh/uv/pull/5794
synced_at: 2026-01-12T16:07:01Z
```

# Add a guide for publishing packages

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/5793

---

_Label `documentation` added by @zanieb on 2024-08-05 16:54_

---

_Label `preview` added by @zanieb on 2024-08-05 16:54_

---

_Marked ready for review by @zanieb on 2024-08-05 17:02_

---

_Comment by @zanieb on 2024-08-05 17:11_

Not a ton to say here, can we include more content?

---

_Review requested from @charliermarsh by @zanieb on 2024-08-05 17:41_

---

_Review requested from @konstin by @zanieb on 2024-08-05 17:41_

---

_@charliermarsh reviewed on 2024-08-05 18:25_

---

_Review comment by @charliermarsh on `docs/guides/publish.md`:9 on 2024-08-05 18:25_

with "the" official?

---

_@charliermarsh reviewed on 2024-08-05 18:25_

---

_Review comment by @charliermarsh on `docs/guides/publish.md`:5 on 2024-08-05 18:25_

"both of which can be accessed via `uvx`."?

---

_@charliermarsh approved on 2024-08-05 18:25_

---

_@zanieb reviewed on 2024-08-05 18:26_

---

_Review comment by @zanieb on `docs/guides/publish.md`:9 on 2024-08-05 18:26_

```suggestion
Build your package with the official `build` frontend:
```

---

_@zanieb reviewed on 2024-08-05 18:27_

---

_Review comment by @zanieb on `docs/guides/publish.md`:5 on 2024-08-05 18:27_

```suggestion
[`twine`](https://github.com/pypa/twine), both of which can be invoked via `uvx`.
```

---

_Merged by @zanieb on 2024-08-05 21:12_

---

_Closed by @zanieb on 2024-08-05 21:12_

---

_Branch deleted on 2024-08-05 21:12_

---

_Comment by @stanmart on 2024-08-07 11:30_

`build` can use `uv` for creating the build environment. The docs for building the package could make use of this. It's much faster, and imo also conceptually nicer than relying on pip for this step only. The downside is that the build command becomes a bit more complex:

```shell
uvx --from build pyproject-build --installer uv
```

---

_Comment by @zanieb on 2024-08-07 12:46_

Oh cool, thank you!

---

_Comment by @zanieb on 2024-08-07 12:48_

https://github.com/astral-sh/uv/pull/5854

---
