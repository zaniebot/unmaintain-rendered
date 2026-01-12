```yaml
number: 8954
title: Docker build workflow does not allow pre-releases
type: issue
state: open
author: zanieb
labels:
  - internal
  - releases
assignees: []
created_at: 2024-11-08T19:24:33Z
updated_at: 2024-11-08T19:24:33Z
url: https://github.com/astral-sh/uv/issues/8954
synced_at: 2026-01-12T15:59:39Z
```

# Docker build workflow does not allow pre-releases

---

_@zanieb_

We can't build pre-release Docker images for uv because there's a tag consistency check 

```
Run version=$(grep "version = " pyproject.toml | sed -e 's/version = "\(.*\)"/\1/g')
The input tag does not match the version from pyproject.toml:
0.5.1-alpha.0
0.5.1a0
Error: Process completed with exit code 1.
```

However, the tags need to be inconsistent for Cargo and Python versioning pre-release schemes

See https://github.com/zanieb/uv/actions/runs/11748226209/job/32731858203

---

_Label `internal` added by @zanieb on 2024-11-08 19:24_

---

_Label `releases` added by @zanieb on 2024-11-08 19:24_

---
