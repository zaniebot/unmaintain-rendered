```yaml
number: 48
title: Consider looking in the on-disk cache for compatible versions
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2023-10-08T00:56:48Z
updated_at: 2023-10-09T02:17:20Z
url: https://github.com/astral-sh/uv/issues/48
synced_at: 2026-01-10T05:40:31Z
```

# Consider looking in the on-disk cache for compatible versions

---

_Issue opened by @charliermarsh on 2023-10-08 00:56_

From the Bun documentation:

> Bun strives to avoid re-downloading packages multiple times. When installing a package, if the cache already contains a version in the range specified by package.json, Bun will use the cached package instead of downloading it again.

> If the semver version has pre-release suffix (1.0.0-beta.0) or a build suffix (1.0.0+20220101), it is replaced with a hash of that value instead, to reduce the chances of errors associated with long file paths.

> When the node_modules folder exists, before installing, Bun checks that node_modules contains all expected packages with appropriate versions. If so bun install completes. Bun uses a custom JSON parser which stops parsing as soon as it finds "name" and "version".

> If a package is missing or has a version incompatible with the package.json, Bun checks for a compatible module in the cache. If found, it is installed into node_modules. Otherwise, the package will be downloaded from the registry then installed.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-08 04:23_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-10-08 18:22_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-08 20:23_

---

_Closed by @charliermarsh on 2023-10-09 02:17_

---
