---
number: 2835
title: E701 check in 0.0.245 disagrees with black for .pyi files
type: issue
state: closed
author: alex
labels:
  - bug
assignees: []
created_at: 2023-02-12T23:35:00Z
updated_at: 2023-02-12T23:56:44Z
url: https://github.com/astral-sh/ruff/issues/2835
synced_at: 2026-01-07T13:12:14-06:00
---

# E701 check in 0.0.245 disagrees with black for .pyi files

---

_Issue opened by @alex on 2023-02-12 23:35_

In `.pyi` files, everything is stubbed out like:

```
def foo() -> int: ...

class MyClass: ...
```

This is how black formats them, but ruff is upset with them.

Example here: https://github.com/pyca/cryptography/actions/runs/4158646856/jobs/7194009425

Thanks

---

_Label `bug` added by @charliermarsh on 2023-02-12 23:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-12 23:36_

---

_Comment by @charliermarsh on 2023-02-12 23:36_

Thanks -- this is resolved for E704 in the next release, but this is good reminder that I need to fix it for E701 and friends too.

---

_Comment by @alex on 2023-02-12 23:43_

Thanks

On Sun, Feb 12, 2023 at 6:37 PM Charlie Marsh ***@***.***> wrote:
>
> Thanks -- this is resolved for E704 in the next release, but this is good reminder that I need to fix it for E701 and friends too.
>
> —
> Reply to this email directly, view it on GitHub, or unsubscribe.
> You are receiving this because you authored the thread.Message ID: ***@***.***>



-- 
All that is necessary for evil to succeed is for good people to do nothing.


---

_Referenced in [astral-sh/ruff#2837](../../astral-sh/ruff/pulls/2837.md) on 2023-02-12 23:50_

---

_Closed by @charliermarsh on 2023-02-12 23:56_

---
