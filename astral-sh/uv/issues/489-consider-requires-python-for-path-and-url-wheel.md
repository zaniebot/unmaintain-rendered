```yaml
number: 489
title: Consider requires-python for path and url wheel
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-11-22T13:08:22Z
updated_at: 2023-12-29T16:08:09Z
url: https://github.com/astral-sh/uv/issues/489
synced_at: 2026-01-10T05:40:31Z
```

# Consider requires-python for path and url wheel

---

_Issue opened by @konstin on 2023-11-22 13:08_

For index wheel, we get the `requires-python` field in the simple api. For path and url wheels, we have to read their metadata to get the `Require-Python` field value, which tells us if this path distribution is even allowed with the current python version.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-22 13:22_

---

_Label `bug` added by @charliermarsh on 2023-11-22 13:37_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-11-22 13:37_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-12-02 00:38_

---

_Comment by @charliermarsh on 2023-12-02 00:48_

Where / when do we need to do this? When building the wheel?

---

_Comment by @charliermarsh on 2023-12-04 04:35_

One weird thing here is: don't we need to build the source distribution to get the `requires-python` value...? Which tells us if we can build that source distribution?

---

_Comment by @konstin on 2023-12-04 16:21_

> Where / when do we need to do this? When building the wheel?

When we see a path or url wheel, we have to reject it if the requires-python doesn't match. Currently, we will install unsupported versions.

> One weird thing here is: don't we need to build the source distribution to get the requires-python value...? Which tells us if we can build that source distribution?

Yes, we have to build the source distribution, or at least call the prepare metadata for build hook, to get that information. I assume this is more of a problem for wheels since i assume for most the source dist build will just fail in this case, but we should still check after the build.

---

_Comment by @konstin on 2023-12-29 16:08_

I think this is better solved by adding `Python` as a virtual dependency

---

_Closed by @konstin on 2023-12-29 16:08_

---
