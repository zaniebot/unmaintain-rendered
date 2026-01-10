```yaml
number: 744
title: Always pre-fetch distribution metadata
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/prefetch
created_at: 2024-01-02T20:59:07Z
updated_at: 2024-01-03T10:37:46Z
url: https://github.com/astral-sh/uv/pull/744
synced_at: 2026-01-10T15:44:44Z
```

# Always pre-fetch distribution metadata

---

_Pull request opened by @charliermarsh on 2024-01-02 20:59_

This PR fixes our prefetching logic to ensure that we always attempt to prefetch the "best-guess" distribution for all dependencies. This logic already existed, but because we only attempted to prefetch when package metadata was available, it almost never triggered. Now, we wait for the package metadata to become available, _then_ kick off the "best-guess" prefetch (for every package).

In my testing, this dramatically improves performance (like 2x). I'm wondering if this regressed at some point?

Closes #743.

---

_Review requested from @konstin by @charliermarsh on 2024-01-02 20:59_

---

_Comment by @charliermarsh on 2024-01-02 20:59_

@konstin - Would you mind generating your same span graphs here? (Also feel free to merge as you like.)

---

_Label `performance` added by @charliermarsh on 2024-01-02 21:04_

---

_@charliermarsh reviewed on 2024-01-02 21:04_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:791 on 2024-01-02 21:04_

This is identical to the `Requires::Dist` case above, but making it recursive requires boxing the async function 👎 

---

_Comment by @konstin on 2024-01-03 10:34_

`black` before:
![black-no-cache](https://github.com/astral-sh/puffin/assets/6826232/dbcf1ec5-d1ef-415e-9890-8c3980c00984)

`black` after: 
![black-no-cache](https://github.com/astral-sh/puffin/assets/6826232/fbc695f0-123c-420b-9b42-72ff5f6c82bb)

`meine_stadt_transparent` before:
![meine_stadt_transparent-no-cache](https://github.com/astral-sh/puffin/assets/6826232/cefeab78-5fdb-44ed-92e4-2def509aa806)

`meine_stadt_transparent` after:
![meine_stadt_transparent-no-cache](https://github.com/astral-sh/puffin/assets/6826232/b77dc2ba-6a6f-49d2-9e1a-473c3ded6dde)

`jupyter` before:
![jupyter-no-cache](https://github.com/astral-sh/puffin/assets/6826232/6ea168ee-1630-4190-afca-091fde1650a7)

`jupyter` after:
![jupyter-no-cache](https://github.com/astral-sh/puffin/assets/6826232/70934f6e-5959-4c5b-9feb-b9da8a2e3028)



---

_@konstin approved on 2024-01-03 10:37_

:rocket: 

---

_Merged by @konstin on 2024-01-03 10:37_

---

_Closed by @konstin on 2024-01-03 10:37_

---

_Branch deleted on 2024-01-03 10:37_

---
