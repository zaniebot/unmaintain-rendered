```yaml
number: 11292
title: "uv-python tests: Use #!/bin/sh instead of #!/bin/bash"
type: pull_request
state: merged
author: geofft
labels:
  - testing
assignees: []
merged: true
base: main
head: bin-sh
created_at: 2025-02-06T19:53:04Z
updated_at: 2025-02-07T15:42:33Z
url: https://github.com/astral-sh/uv/pull/11292
synced_at: 2026-01-12T16:09:46Z
```

# uv-python tests: Use #!/bin/sh instead of #!/bin/bash

---

_@geofft_

NixOS has (and POSIX mandates) a /bin/sh but not a /bin/bash, so this fixes tests on NixOS.


---

_Review requested from @zanieb by @geofft on 2025-02-06 19:53_

---

_@zanieb approved on 2025-02-06 19:53_

---

_Label `testing` added by @zanieb on 2025-02-06 19:54_

---

_Merged by @zanieb on 2025-02-07 15:42_

---

_Closed by @zanieb on 2025-02-07 15:42_

---
