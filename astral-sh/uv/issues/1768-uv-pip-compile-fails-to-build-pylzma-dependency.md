```yaml
number: 1768
title: uv pip compile fails to build pylzma dependency
type: issue
state: closed
author: carno
labels:
  - bug
assignees: []
created_at: 2024-02-20T17:19:42Z
updated_at: 2024-02-20T20:37:07Z
url: https://github.com/astral-sh/uv/issues/1768
synced_at: 2026-01-10T05:40:31Z
```

# uv pip compile fails to build pylzma dependency

---

_Issue opened by @carno on 2024-02-20 17:19_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
As far as I understand this is more of a `pylzma` issue, but as it works with `pip-compile` I will leave it here in case it's something worth a workaround aka "correction for a common mistake" as mentioned in https://github.com/astral-sh/uv/issues/1528#issuecomment-1949410198

If I'm not mistaken, the culprit is in how `pylzma` generates it's version when being built locally: https://github.com/fancycode/pylzma/blob/master/version.py

As I run `uv pip compile` in Docker, in a gitlab runner, it adds hash of my repository to the version string (at least the part after `-g` matches that)…

```
$ uv --version
uv 0.1.5
$ uv pip compile --output-file=- images/common/requirements.in
error: Failed to download and build: pylzma==0.5.0
  Caused by: Failed to build: pylzma==0.5.0
  Caused by: Build backend failed to determine extra requires with `build_wheel()`:
--- stdout:
--- stderr:
Traceback (most recent call last):
  File "<string>", line 10, in <module>
  File "/builds/redacted/.cache/uv/.tmpHEBm3b/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=['wheel'])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/builds/redacted/.cache/uv/.tmpHEBm3b/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
    self.run_setup()
  File "/builds/redacted/.cache/uv/.tmpHEBm3b/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 480, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/builds/redacted/.cache/uv/.tmpHEBm3b/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 189, in <module>
  File "/builds/redacted/.cache/uv/.tmpHEBm3b/.venv/lib/python3.11/site-packages/setuptools/__init__.py", line 103, in setup
    return distutils.core.setup(**attrs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/builds/redacted/.cache/uv/.tmpHEBm3b/.venv/lib/python3.11/site-packages/setuptools/_distutils/core.py", line 147, in setup
    _setup_distribution = dist = klass(attrs)
                                 ^^^^^^^^^^^^
  File "/builds/redacted/.cache/uv/.tmpHEBm3b/.venv/lib/python3.11/site-packages/setuptools/dist.py", line 314, in __init__
    self.metadata.version = self._normalize_version(self.metadata.version)
                            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/builds/redacted/.cache/uv/.tmpHEBm3b/.venv/lib/python3.11/site-packages/setuptools/dist.py", line 350, in _normalize_version
    normalized = str(Version(version))
                     ^^^^^^^^^^^^^^^^
  File "/builds/redacted/.cache/uv/.tmpHEBm3b/.venv/lib/python3.11/site-packages/setuptools/_vendor/packaging/version.py", line 198, in __init__
    raise InvalidVersion(f"Invalid version: '{version}'")
setuptools.extern.packaging.version.InvalidVersion: Invalid version: '15.0.0-744-gb2d6'
```

---

_Comment by @charliermarsh on 2024-02-20 19:06_

My guess is that pip does the build in a temporary directory outside of the project hierarchy, whereas we're doing it within a temporary directory in the `.venv`?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-20 19:37_

---

_Label `bug` added by @charliermarsh on 2024-02-20 19:37_

---

_Comment by @charliermarsh on 2024-02-20 19:37_

I consider this a bug though. I've seen it in other packages too.

---

_Comment by @charliermarsh on 2024-02-20 20:23_

Fixed in https://github.com/astral-sh/uv/pull/1782.

---

_Closed by @charliermarsh on 2024-02-20 20:37_

---
