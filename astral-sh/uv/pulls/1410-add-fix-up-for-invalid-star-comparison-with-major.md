```yaml
number: 1410
title: Add fix-up for invalid star comparison with major-only version
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/req-ii
created_at: 2024-02-16T02:04:45Z
updated_at: 2024-02-16T02:12:17Z
url: https://github.com/astral-sh/uv/pull/1410
synced_at: 2026-01-10T15:33:24Z
```

# Add fix-up for invalid star comparison with major-only version

---

_Pull request opened by @charliermarsh on 2024-02-16 02:04_

## Summary

Closes https://github.com/astral-sh/uv/issues/1402.

## Test Plan

Ran `cargo run pip install junos-eznc==2.6.5`, which still fails for me, but fails identically to `pip` (and not on the `requires-python`):

```
/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmp7mxT9L/built-wheels-v0/pypi/ncclient/0.6.13/4vvPwmDC_CL2OUXd68Zqb/ncclient-0.6.13.tar.gz/versioneer.py:421: SyntaxWarning: invalid escape sequence '\s'
  LONG_VERSION_PY['git'] = '''
Traceback (most recent call last):
  File "<string>", line 10, in <module>
  File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmplD5mMO/.venv/lib/python3.12/site-packages/setuptools/build_meta.py", line 366, in prepare_metadata_for_build_wheel
    self.run_setup()
  File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmplD5mMO/.venv/lib/python3.12/site-packages/setuptools/build_meta.py", line 480, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmplD5mMO/.venv/lib/python3.12/site-packages/setuptools/build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 45, in <module>
  File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmp7mxT9L/built-wheels-v0/pypi/ncclient/0.6.13/4vvPwmDC_CL2OUXd68Zqb/ncclient-0.6.13.tar.gz/versioneer.py", line 1480, in get_version
    return get_versions()["version"]
           ^^^^^^^^^^^^^^
  File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmp7mxT9L/built-wheels-v0/pypi/ncclient/0.6.13/4vvPwmDC_CL2OUXd68Zqb/ncclient-0.6.13.tar.gz/versioneer.py", line 1412, in get_versions
    cfg = get_config_from_root(root)
          ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmp7mxT9L/built-wheels-v0/pypi/ncclient/0.6.13/4vvPwmDC_CL2OUXd68Zqb/ncclient-0.6.13.tar.gz/versioneer.py", line 342, in get_config_from_root
    parser = configparser.SafeConfigParser()
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
AttributeError: module 'configparser' has no attribute 'SafeConfigParser'. Did you mean: 'RawConfigParser'?
```

---

_Label `bug` added by @charliermarsh on 2024-02-16 02:04_

---

_@zanieb approved on 2024-02-16 02:10_

---

_Merged by @charliermarsh on 2024-02-16 02:12_

---

_Closed by @charliermarsh on 2024-02-16 02:12_

---

_Branch deleted on 2024-02-16 02:12_

---

_Branch restored on 2024-02-16 02:12_

---

_Branch deleted on 2024-02-16 02:12_

---
