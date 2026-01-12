```yaml
number: 4018
title: "Allow system interpreters set by `VIRTUAL_ENV` to be used without opt-in"
type: pull_request
state: closed
author: zanieb
labels:
  - virtualenv
assignees: []
base: main
head: zb/virtual-env-explicit
created_at: 2024-06-04T17:50:59Z
updated_at: 2024-06-12T14:24:06Z
url: https://github.com/astral-sh/uv/pull/4018
synced_at: 2026-01-12T16:05:59Z
```

# Allow system interpreters set by `VIRTUAL_ENV` to be used without opt-in

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/3905

Allows an active virtual environment to be used even if it's not PEP 405 compliant (unless `SystemPython::Disallowed` is passed; which I don't think we _ever_ do right now in user-facing code). User reports indicate that PEP 405 compliance may be too high of a bar. We will emit a debug log noting this, but otherwise not warn — we may want a more user-facing warning.

---

_Renamed from "Treat interpreters from `VIRTUAL_ENV` as explicit selection of a "system" interpreter" to "Allow system interpreters to be used when from `VIRTUAL_ENV`" by @zanieb on 2024-06-04 17:55_

---

_Label `virtualenv` added by @zanieb on 2024-06-04 18:10_

---

_Renamed from "Allow system interpreters to be used when from `VIRTUAL_ENV`" to "Allow system interpreters set by `VIRTUAL_ENV` to be used without opt-in" by @zanieb on 2024-06-04 18:25_

---

_Marked ready for review by @zanieb on 2024-06-04 18:25_

---

_Comment by @zanieb on 2024-06-04 18:25_

I'm a little hesitant here, but it seems common enough to allow it?

---

_Review requested from @charliermarsh by @zanieb on 2024-06-04 18:25_

---

_Comment by @zanieb on 2024-06-04 18:56_

I thought this included #4009 but it actually doesn't resolve that. I'm leaning towards waiting on this now.

---

_Closed by @zanieb on 2024-06-12 14:24_

---
