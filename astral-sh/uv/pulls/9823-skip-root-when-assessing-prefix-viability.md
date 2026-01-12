```yaml
number: 9823
title: Skip root when assessing prefix viability
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/r
created_at: 2024-12-11T19:45:43Z
updated_at: 2024-12-11T19:59:50Z
url: https://github.com/astral-sh/uv/pull/9823
synced_at: 2026-01-12T16:08:59Z
```

# Skip root when assessing prefix viability

---

_@charliermarsh_

## Summary

In CPython, it appears that `/` is not considered as a valid path in `search_up`:

```c
static PyObject *
getpath_dirname(PyObject *Py_UNUSED(self), PyObject *args)
{
    PyObject *path;
    if (!PyArg_ParseTuple(args, "U", &path)) {
        return NULL;
    }
    Py_ssize_t end = PyUnicode_GET_LENGTH(path);
    Py_ssize_t pos = PyUnicode_FindChar(path, SEP, 0, end, -1);
    if (pos < 0) {
        return PyUnicode_FromStringAndSize(NULL, 0);
    }
    return PyUnicode_Substring(path, 0, pos);
}
```

```python
def search_up(prefix, *landmarks, test=isfile):
    while prefix:
        if any(test(joinpath(prefix, f)) for f in landmarks):
            return prefix
        prefix = dirname(prefix)
```

Closes https://github.com/astral-sh/uv/issues/9818.


---

_Label `bug` added by @charliermarsh on 2024-12-11 19:45_

---

_Marked ready for review by @charliermarsh on 2024-12-11 19:45_

---

_Review requested from @konstin by @charliermarsh on 2024-12-11 19:45_

---

_@zanieb approved on 2024-12-11 19:48_

---

_Merged by @charliermarsh on 2024-12-11 19:59_

---

_Closed by @charliermarsh on 2024-12-11 19:59_

---

_Branch deleted on 2024-12-11 19:59_

---
