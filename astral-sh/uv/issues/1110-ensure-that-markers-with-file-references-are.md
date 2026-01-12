```yaml
number: 1110
title: Ensure that markers with file references are parsed correctly
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-01-25T21:58:17Z
updated_at: 2024-01-26T01:41:51Z
url: https://github.com/astral-sh/uv/issues/1110
synced_at: 2026-01-12T15:58:25Z
```

# Ensure that markers with file references are parsed correctly

---

_@charliermarsh_

Apparently this is fine:

```
Flask; os_name="windows"
```

But this is not:
```
Flask @ git://; os_name="windows"
```

Instead, you need to do:
```
Flask @ git:// ; os_name="windows"
```

---

_Label `bug` added by @charliermarsh on 2024-01-25 21:58_

---

_Comment by @charliermarsh on 2024-01-25 21:58_

I don't know if we actually suffer from this bug, it's something Armin mentioned. Need to verify.

---

_Comment by @charliermarsh on 2024-01-26 01:41_

Confirmed that we do parse these correctly. Checking if we _generate_ them anywhere...

---

_Comment by @charliermarsh on 2024-01-26 01:41_

It looks like we always insert a space in the display, so this should be fine.

---

_Closed by @charliermarsh on 2024-01-26 01:41_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-26 01:41_

---
