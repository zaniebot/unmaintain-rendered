---
number: 12443
title: "UV can't resolve correct dependencies of package, but pip can"
type: issue
state: closed
author: ethan-tonic
labels:
  - bug
assignees: []
created_at: 2025-03-24T17:05:05Z
updated_at: 2025-03-24T18:57:08Z
url: https://github.com/astral-sh/uv/issues/12443
synced_at: 2026-01-10T01:25:19Z
---

# UV can't resolve correct dependencies of package, but pip can

---

_Issue opened by @ethan-tonic on 2025-03-24 17:05_

### Summary

When running pip on Python 3.11.11, I can run `pip install bertopic` and it will resolve to installing the following
```
MarkupSafe-3.0.2 Pillow-11.1.0
bertopic-0.17.0 certifi-2025.1.31
charset-normalizer-3.4.1 filelock-3.18.0
fsspec-2025.3.0 hdbscan-0.8.40
huggingface-hub-0.29.3 idna-3.10
jinja2-3.1.6 joblib-1.4.2
llvmlite-0.44.0 mpmath-1.3.0
narwhals-1.32.0 networkx-3.4.2
numba-0.61.0 numpy-2.1.3
packaging-24.2 pandas-2.2.3
plotly-6.0.1 pynndescent-0.5.13
python-dateutil-2.9.0.post0 pytz-2025.1
pyyaml-6.0.2 regex-2024.11.6
requests-2.32.3 safetensors-0.5.3
scikit-learn-1.6.1 scipy-1.15.2
sentence-transformers-3.4.1 six-1.17.0
sympy-1.13.1 threadpoolctl-3.6.0
tokenizers-0.21.1 torch-2.6.0
tqdm-4.67.1 transformers-4.50.0
typing-extensions-4.12.2 tzdata-2025.2
umap-learn-0.5.7 urllib3-2.3.0
```
Meanwhile, if I run `uv venv --python 3.11.11` followed by `uv pip install bertopic`, I get the following
```
Resolved 43 packages in 106ms
  × Failed to build `numba==0.53.1`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 14, in <module>
        File "/Users/ethan/.cache/uv/builds-v0/.tmp9S2tyL/lib/python3.11/site-packages/setuptools/build_meta.py", line 334, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/ethan/.cache/uv/builds-v0/.tmp9S2tyL/lib/python3.11/site-packages/setuptools/build_meta.py", line 304, in _get_build_requires
          self.run_setup()
        File "/Users/ethan/.cache/uv/builds-v0/.tmp9S2tyL/lib/python3.11/site-packages/setuptools/build_meta.py", line 522, in run_setup
          super().run_setup(setup_script=setup_script)
        File "/Users/ethan/.cache/uv/builds-v0/.tmp9S2tyL/lib/python3.11/site-packages/setuptools/build_meta.py", line 320, in run_setup
          exec(code, locals())
        File "<string>", line 50, in <module>
        File "<string>", line 47, in _guard_py_ver
      RuntimeError: Cannot install on Python version 3.11.11; only versions >=3.6,<3.10 are supported.

      hint: This usually indicates a problem with the package or the build environment.
  help: `numba` (v0.53.1) was included because `bertopic` (v0.17.0) depends on `umap-learn` (v0.5.7) which depends on `numba`
```

When running this command again with verbose mode, it seems that uv is choosing `numpy==2.2.4` instead of `numpy==2.1.3` which is causing it to go to an older version of numba and thus making the install fail. It seems the dependency resolution of uv isn't working correctly and is thus resolving to the wrong package versions. Interestingly, when running `uv pip install bertopic --dry-run` it doesn't detect that the numba version is invalid for the given python version. This might be why uv is missing the correct dependency version.

### Platform

macOS 15 Arm

### Version

uv 0.6.9 (3d9460278 2025-03-20)

### Python version

Python 3.11.11

---

_Label `bug` added by @ethan-tonic on 2025-03-24 17:05_

---

_Comment by @notatallshaw on 2025-03-24 17:15_

Same issue as https://github.com/astral-sh/uv/issues/12060

As a workaround you could provide `--only-binary "numba"`

---

_Comment by @x0rw on 2025-03-24 17:29_

Upgrading llvmlite separately should work `uv pip install --upgrade llvmlite numba`

---

_Closed by @charliermarsh on 2025-03-24 18:57_

---
