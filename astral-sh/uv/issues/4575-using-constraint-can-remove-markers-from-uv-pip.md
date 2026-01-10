---
number: 4575
title: "Using --constraint can remove markers from 'uv pip compile --universal'"
type: issue
state: closed
author: Lordshinjo
labels:
  - bug
  - lock
assignees: []
created_at: 2024-06-27T08:46:21Z
updated_at: 2024-06-29T16:51:05Z
url: https://github.com/astral-sh/uv/issues/4575
synced_at: 2026-01-10T01:23:39Z
---

# Using --constraint can remove markers from 'uv pip compile --universal'

---

_Issue opened by @Lordshinjo on 2024-06-27 08:46_

(Note: I suspect the issues also applies to `--no-strip-markers`, but am only talking about `--universal` here)

It looks like constraints are considered equal as regular requirements during marker propagation, but constraints do not always contain environment markers (though they can).
This means that when declaring a constraint with no marker on a package, that package and all its transitive dependencies end up stripped of their markers.

AFAIU markers on constraints should be applied but should not result in making markers more open (such as removing the markers or adding a "or").

The following is a bit of a silly example, but the issue applies nonetheless.

`requirements.in`:
```
requests; platform_system == "Windows"
```

`constraints.txt`:
```
requests==2.32.3
```

Running the command `uv pip compile --universal -c constraints.txt -o requirements.txt requirements.in`, we get
* Actual result ([log](https://gist.githubusercontent.com/Lordshinjo/6edc9ccae94348f6d7490b1de338c854/raw/b3c78e56781307c90c735552bd326465d620d2d0/with-constraints.txt)):
```
certifi==2024.6.2
    # via requests
charset-normalizer==3.3.2
    # via requests
idna==3.7
    # via requests
requests==2.32.3
    # via
    #   -c constraints.txt
    #   -r requirements.in
urllib3==2.2.2
    # via requests
```
* Expected result ([log](https://gist.githubusercontent.com/Lordshinjo/6edc9ccae94348f6d7490b1de338c854/raw/b3c78e56781307c90c735552bd326465d620d2d0/without-constraints.txt)) (which can be obtained running without the `-c constraints.txt`):
```
certifi==2024.6.2 ; platform_system == 'Windows'
    # via requests
charset-normalizer==3.3.2 ; platform_system == 'Windows'
    # via requests
idna==3.7 ; platform_system == 'Windows'
    # via requests
requests==2.32.3 ; platform_system == 'Windows'
    # via -r requirements.in
urllib3==2.2.2 ; platform_system == 'Windows'
    # via requests
```

---

_Label `bug` added by @zanieb on 2024-06-27 11:03_

---

_Label `lock` added by @zanieb on 2024-06-27 11:11_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-28 21:10_

---

_Comment by @charliermarsh on 2024-06-28 21:10_

Nice issue, thank you.

---

_Referenced in [astral-sh/uv#4648](../../astral-sh/uv/pulls/4648.md) on 2024-06-29 16:39_

---

_Closed by @charliermarsh on 2024-06-29 16:51_

---
