```yaml
number: 388
title: Consider requires-python before selecting a source distribution
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-11-10T14:34:50Z
updated_at: 2023-11-13T16:14:09Z
url: https://github.com/astral-sh/uv/issues/388
synced_at: 2026-01-10T05:40:31Z
```

# Consider requires-python before selecting a source distribution

---

_Issue opened by @konstin on 2023-11-10 14:34_

Currently installing pandas fails on python 3.8 because we're trying to install pandas 2.1.2 from pandas-2.1.2.tar.gz, but pandas only support python 3.9+ in this version, so we fail trying to build the sdist, while pip directly picks pandas 1.24.4 which even has a wheel

---

_Added to milestone `Future` by @konstin on 2023-11-10 14:34_

---

_Label `bug` added by @charliermarsh on 2023-11-10 14:37_

---

_Comment by @charliermarsh on 2023-11-10 14:38_

I assume this is the `requires-python` in the file metadata...?

---

_Removed from milestone `Future` by @charliermarsh on 2023-11-10 14:38_

---

_Added to milestone `Feature complete` by @charliermarsh on 2023-11-10 14:38_

---

_Comment by @konstin on 2023-11-10 14:40_

No, it's the requires-python in the api; I think pip looks at `data-requires-python` in https://pypi.org/simple/pandas/, in the json format it's called `requires-python`

---

_Comment by @charliermarsh on 2023-11-10 14:43_

I think we're talking about the same thing, I was referring to the `requires-python` on the `File` struct which comes from the simple API.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-10 14:51_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-11-10 15:00_

---

_Comment by @charliermarsh on 2023-11-10 15:01_

Likely requires changes to both the resolver (where we build the version map) and the installer (in the distribution finder).

---

_Closed by @konstin on 2023-11-13 16:14_

---
