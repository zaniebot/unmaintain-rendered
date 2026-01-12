```yaml
number: 579
title: "Failure to install `tomli` from source distribution"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-12-06T19:39:32Z
updated_at: 2023-12-13T15:11:03Z
url: https://github.com/astral-sh/uv/issues/579
synced_at: 2026-01-12T15:58:24Z
```

# Failure to install `tomli` from source distribution

---

_@charliermarsh_

Given `tomli @ https://files.pythonhosted.org/packages/c0/3f/d7af728f075fb08564c5949a9c95e44352e23dee646869fa104a3b2060a3/tomli-2.0.1.tar.gz`:

```
error: Failed to download distributions
  Caused by: Failed to build: tomli @ https://files.pythonhosted.org/packages/c0/3f/d7af728f075fb08564c5949a9c95e44352e23dee646869fa104a3b2060a3/tomli-2.0.1.tar.gz
  Caused by: Build backend failed to build wheel through `build_wheel()`:
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 6, in <module>
  File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmp3fbPE1/.venv/lib/python3.10/site-packages/flit_core/buildapi.py", line 72, in build_wheel
    info = make_wheel_in(pyproj_toml, Path(wheel_directory))
  File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmp3fbPE1/.venv/lib/python3.10/site-packages/flit_core/wheel.py", line 224, in make_wheel_in
    wb.build(editable)
  File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmp3fbPE1/.venv/lib/python3.10/site-packages/flit_core/wheel.py", line 210, in build
    self.copy_module()
  File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmp3fbPE1/.venv/lib/python3.10/site-packages/flit_core/wheel.py", line 164, in copy_module
    self._add_file(full_path, rel_path)
  File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmp3fbPE1/.venv/lib/python3.10/site-packages/flit_core/wheel.py", line 111, in _add_file
    zinfo = zipfile.ZipInfo.from_file(full_path, rel_path)
  File "/Users/crmarsh/.local/share/rtx/installs/python/3.10.13/lib/python3.10/zipfile.py", line 520, in from_file
    zinfo = cls(arcname, date_time)
  File "/Users/crmarsh/.local/share/rtx/installs/python/3.10.13/lib/python3.10/zipfile.py", line 364, in __init__
    raise ValueError('ZIP does not support timestamps before 1980')
ValueError: ZIP does not support timestamps before 1980
---
```

---

_Label `bug` added by @charliermarsh on 2023-12-06 19:39_

---

_Comment by @charliermarsh on 2023-12-06 19:39_

`pip` installs without error.

---

_Comment by @charliermarsh on 2023-12-12 21:54_

@konstin - Did you have a chance to look at this one?

---

_Comment by @konstin on 2023-12-13 11:08_

Filed upstream: https://github.com/alexcrichton/tar-rs/issues/349

---

_Closed by @konstin on 2023-12-13 15:11_

---
