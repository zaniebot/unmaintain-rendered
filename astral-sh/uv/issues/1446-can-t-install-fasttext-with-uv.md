```yaml
number: 1446
title: "Can't install fasttext with uv"
type: issue
state: closed
author: ivanychev
labels:
  - bug
assignees: []
created_at: 2024-02-16T07:17:54Z
updated_at: 2024-02-21T13:26:28Z
url: https://github.com/astral-sh/uv/issues/1446
synced_at: 2026-01-12T15:58:27Z
```

# Can't install fasttext with uv

---

_@ivanychev_

Reproduce:

```bash
python -m venv venv  # Python 3.10
source venv/bin/activate
uv pip install fasttext==0.9.2
```

Error:

```
error: Failed to download and build: fasttext==0.9.2
  Caused by: Failed to build: fasttext==0.9.2
  Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_wheel`:
--- stdout:

--- stderr:
/private/var/folders/m0/mgh9dmxj53s9ytmy_m0zvfqm0000gn/T/.tmpZAGWL2/.venv/bin/python: No module named pip
Traceback (most recent call last):
  File "<string>", line 38, in __init__
ModuleNotFoundError: No module named 'pybind11'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<string>", line 10, in <module>
  File "/private/var/folders/m0/mgh9dmxj53s9ytmy_m0zvfqm0000gn/T/.tmpZAGWL2/.venv/lib/python3.10/site-packages/setuptools/build_meta.py", line 366, in prepare_metadata_for_build_wheel
    self.run_setup()
  File "/private/var/folders/m0/mgh9dmxj53s9ytmy_m0zvfqm0000gn/T/.tmpZAGWL2/.venv/lib/python3.10/site-packages/setuptools/build_meta.py", line 480, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/private/var/folders/m0/mgh9dmxj53s9ytmy_m0zvfqm0000gn/T/.tmpZAGWL2/.venv/lib/python3.10/site-packages/setuptools/build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 72, in <module>
  File "<string>", line 41, in __init__
RuntimeError: pybind11 install failed.
—
```

---

_Comment by @ivanychev on 2024-02-16 07:19_

FYI,

```
uv pip install pybind11
uv pip install fasttext==0.9.2
```

also fails with the same error

---

_Label `bug` added by @dhruvmanila on 2024-02-16 07:24_

---

_Comment by @charliermarsh on 2024-02-16 16:19_

I think the problem here is that `fasttext` sort of "secretly" depends on pip: https://github.com/facebookresearch/fastText/blob/b733943e84263f432fa47588643822194ee03dd1/setup.py#L40. But it's not declared as a build dependency.

---

_Comment by @charliermarsh on 2024-02-16 16:19_

This may need to be fixed in `fasttext` itself, or we'd have to special-case `fasttext` in uv.

---

_Comment by @konstin on 2024-02-21 13:24_

This has been fixed on fasttext's side by adding a pyproject.toml, `uv pip install "fasttext @ git+https://github.com/facebookresearch/fastText"` installs succesfully.

---

_Closed by @charliermarsh on 2024-02-21 13:26_

---
