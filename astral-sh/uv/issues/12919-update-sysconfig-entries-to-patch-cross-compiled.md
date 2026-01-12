```yaml
number: 12919
title: "Update sysconfig entries to patch cross-compiled `CC` paths"
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2025-04-16T13:35:04Z
updated_at: 2025-06-02T15:58:32Z
url: https://github.com/astral-sh/uv/issues/12919
synced_at: 2026-01-12T16:01:15Z
```

# Update sysconfig entries to patch cross-compiled `CC` paths

---

_@zanieb_

#12239 fixed this for `aarch64`, but this is also going to be a problem for other cross-compiles like, s390x, e.g., the values will be:

```
  host_cc: /usr/bin/x86_64-linux-gnu-gcc
  host_cxx: /usr/bin/x86_64-linux-gnu-g++
  target_cc: /usr/bin/s390x-linux-gnu-gcc
```

_Originally posted by @zanieb in https://github.com/astral-sh/uv/issues/12239#issuecomment-2729651511_
            

---

_Comment by @zanieb on 2025-04-16 13:35_

@samypr100 were you interested in making this more robust?

---

_Label `bug` added by @zanieb on 2025-04-16 14:51_

---

_Closed by @zanieb on 2025-06-02 15:58_

---
