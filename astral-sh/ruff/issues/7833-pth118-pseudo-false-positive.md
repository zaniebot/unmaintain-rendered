```yaml
number: 7833
title: "PTH118: pseudo false positive"
type: issue
state: closed
author: rassie
labels:
  - bug
assignees: []
created_at: 2023-10-06T07:53:55Z
updated_at: 2023-10-09T12:04:37Z
url: https://github.com/astral-sh/ruff/issues/7833
synced_at: 2026-01-10T11:09:50Z
```

# PTH118: pseudo false positive

---

_Issue opened by @rassie on 2023-10-06 07:53_

(this is an extremely minor issue, feel free to ignore / close / etc.)

Currently, `ruff` recommends using `Path` and `/` separator for basically anything that uses `os.path.join`. `Path.joinpath` also appears [elsewhere](https://github.com/astral-sh/ruff/discussions/7036), but is not on the forefront.

However, sometimes path parts are splatted:

```
key = "a:b:c:d"
os.path.join(directory, *key.split(":"))
```

In this case, using `/` is not viable. I would argue that the message for that case should be changed to explicitely recommend using `joinpath`.

---

_Label `bug` added by @charliermarsh on 2023-10-08 14:37_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-08 14:40_

---

_Comment by @charliermarsh on 2023-10-08 14:47_

Easy fix, why not :)

---

_Comment by @rassie on 2023-10-08 14:48_

Thanks!

---

_Closed by @charliermarsh on 2023-10-09 12:04_

---
