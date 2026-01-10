```yaml
number: 5805
title: Strip URL fragments from lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/lock-remove-hash
created_at: 2024-08-05T23:58:19Z
updated_at: 2024-08-06T14:00:40Z
url: https://github.com/astral-sh/uv/pull/5805
synced_at: 2026-01-10T13:31:54Z
```

# Strip URL fragments from lockfile

---

_Pull request opened by @charliermarsh on 2024-08-05 23:58_

## Summary

I noticed that we write entries like:

```
sdist = { url = "https://pkgs.dev.azure.com/astral-sh/_packaging/db3d73a9-2c37-40f9-8680-515ec6e132c4/pypi/download/iniconfig/2/iniconfig-2.0.0.tar.gz#sha256=2d91e135bf72d31a410b17c16da610a82cb55f6b0477d1a902134b24a455b8b3", hash = "sha256:2d91e135bf72d31a410b17c16da610a82cb55f6b0477d1a902134b24a455b8b3" }
```

(For registries that include hashes in the URLs, that is.)

This PR drops those unnecessary components from the lockfile (notice that the hash is repeated).


---

_Label `internal` added by @charliermarsh on 2024-08-05 23:58_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-05 23:58_

---

_@zanieb approved on 2024-08-06 02:28_

---

_@ibraheemdev approved on 2024-08-06 13:16_

---

_Merged by @charliermarsh on 2024-08-06 14:00_

---

_Closed by @charliermarsh on 2024-08-06 14:00_

---

_Branch deleted on 2024-08-06 14:00_

---
