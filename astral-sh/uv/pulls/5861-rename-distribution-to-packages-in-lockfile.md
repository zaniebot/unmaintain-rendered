```yaml
number: 5861
title: "Rename `distribution` to `packages` in lockfile"
type: pull_request
state: merged
author: konstin
labels:
  - preview
  - lock
assignees: []
merged: true
base: main
head: konsti/distribution-to-package
created_at: 2024-08-07T13:57:22Z
updated_at: 2024-08-08T15:25:08Z
url: https://github.com/astral-sh/uv/pull/5861
synced_at: 2026-01-12T16:07:03Z
```

# Rename `distribution` to `packages` in lockfile

---

_@konstin_

Currently, the entry for a package+version+source table is called `distribution`. That is incorrect, the `sdist` and `wheel` fields inside of that table are distributions, the table itself is for a package. We also align ourselves closer with PEP 751.

I went through `lock.rs` and renamed all occurrences of "distribution" that actually referred to a "package".

This change invalidates all existing lockfiles.

Bikeshedding: Do we call it `package` or `packages`? See also https://github.com/python/peps/pull/3877

`package` is nice because it looks like a header:

```toml
[[package]]
name = "anyio"
version = "4.3.0"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "idna" },
    { name = "sniffio" },
]
sdist = { url = "https://files.pythonhosted.org/packages/db/4d/3970183622f0330d3c23d9b8a5f52e365e50381fd484d08e3285104333d3/anyio-4.3.0.tar.gz", hash = "sha256:f75253795a87df48568485fd18cdd2a3fa5c4f7c5be8e5e36637733fce06fed6", size = 159642 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl", hash = "sha256:048e05d0f6caeed70d731f3db756d35dcc1f35747c8c403364a8332c630441b8", size = 85584 },
]
```

`packages` is nice because the field is not a single entry, but a list.

2/3 for https://github.com/astral-sh/uv/issues/4893

---

_Label `preview` added by @konstin on 2024-08-07 13:57_

---

_Label `lock` added by @konstin on 2024-08-07 13:57_

---

_Comment by @zanieb on 2024-08-07 14:03_

What's the migration story here? Are we just throwing an error or will we replace the lockfile?

---

_Comment by @konstin on 2024-08-07 14:04_

Yeah

---

_Comment by @zanieb on 2024-08-07 14:07_

What?

---

_Comment by @konstin on 2024-08-07 14:17_

Ah sorry i misread; we never error with invalid lockfiles, we ignore them and replace them. We could add an alias or something if we want to preserve previous preferences.

---

_Comment by @zanieb on 2024-08-07 14:18_

I think that's okay with me, thank you.

---

_Comment by @charliermarsh on 2024-08-07 14:41_

Can we add a basic test to verify that we don't error for an invalid lockfile?

---

_@charliermarsh approved on 2024-08-07 14:45_

---

_Comment by @charliermarsh on 2024-08-07 18:16_

Will add and merge.

---

_Comment by @konstin on 2024-08-08 08:25_

I've added an alias and confirmed that we read the previous lock as preferences

---

_Comment by @charliermarsh on 2024-08-08 12:20_

Cool. I think this is fine to merge as-is.

---

_Merged by @charliermarsh on 2024-08-08 15:25_

---

_Closed by @charliermarsh on 2024-08-08 15:25_

---

_Branch deleted on 2024-08-08 15:25_

---
