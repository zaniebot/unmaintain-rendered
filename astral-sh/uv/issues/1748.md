```yaml
number: 1748
title: "Source installs fail due to timestamps/zip for `flit`-based sdists"
type: issue
state: closed
author: adrianeboyd
labels: []
assignees: []
created_at: 2024-02-20T11:54:57Z
updated_at: 2024-02-20T14:54:30Z
url: https://github.com/astral-sh/uv/issues/1748
synced_at: 2026-01-10T05:40:31Z
```

# Source installs fail due to timestamps/zip for `flit`-based sdists

---

_Issue opened by @adrianeboyd on 2024-02-20 11:54_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

(Some?) sdists created with `flit` (e.g. `cloudpathlib`, `tomli`, `typer`) include a `PKG-INFO` file with a timestamp of Jan 1 1970, which raises an error when building from source. `pip` installs without errors.

Command, using `uv` 0.1.5:

```shell
uv pip install cloudpathlib==0.17.0 --no-binary cloudpathlib --no-cache --reinstall
```

Output:

```none
Resolved 2 packages in 392ms
error: Failed to download distributions
  Caused by: Failed to fetch wheel: cloudpathlib==0.17.0
  Caused by: Failed to build: cloudpathlib==0.17.0
  Caused by: Build backend failed to build wheel through `build_wheel()`:
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 6, in <module>
  File "/tmp/.tmplPwcVh/.tmp6SwAEy/.venv/lib/python3.8/site-packages/flit_core/buildapi.py", line 72, in build_wheel
    info = make_wheel_in(pyproj_toml, Path(wheel_directory))
  File "/tmp/.tmplPwcVh/.tmp6SwAEy/.venv/lib/python3.8/site-packages/flit_core/wheel.py", line 224, in make_wheel_in
    wb.build(editable)
  File "/tmp/.tmplPwcVh/.tmp6SwAEy/.venv/lib/python3.8/site-packages/flit_core/wheel.py", line 210, in build
    self.copy_module()
  File "/tmp/.tmplPwcVh/.tmp6SwAEy/.venv/lib/python3.8/site-packages/flit_core/wheel.py", line 164, in copy_module
    self._add_file(full_path, rel_path)
  File "/tmp/.tmplPwcVh/.tmp6SwAEy/.venv/lib/python3.8/site-packages/flit_core/wheel.py", line 111, in _add_file
    zinfo = zipfile.ZipInfo.from_file(full_path, rel_path)
  File "/usr/lib/python3.8/zipfile.py", line 539, in from_file
    zinfo = cls(arcname, date_time)
  File "/usr/lib/python3.8/zipfile.py", line 362, in __init__
    raise ValueError('ZIP does not support timestamps before 1980')
ValueError: ZIP does not support timestamps before 1980
```

This looks like the same issue as #579, but it doesn't appear to be fixed by #634?

---

_Assigned to @konstin by @konstin on 2024-02-20 12:07_

---

_Comment by @konstin on 2024-02-20 12:28_

This got lost in the streaming unpack implementation, i added a test to prevent this from happening again.

---

_Closed by @charliermarsh on 2024-02-20 14:54_

---
