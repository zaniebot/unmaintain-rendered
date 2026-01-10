```yaml
number: 10772
title: uv 0.5.21 fails to install pytorch
type: issue
state: closed
author: kasperipalkama
labels:
  - bug
assignees: []
created_at: 2025-01-20T07:32:17Z
updated_at: 2025-01-20T17:23:09Z
url: https://github.com/astral-sh/uv/issues/10772
synced_at: 2026-01-10T04:27:58Z
```

# uv 0.5.21 fails to install pytorch

---

_Issue opened by @kasperipalkama on 2025-01-20 07:32_

I have configured pytorch installation in pyproject toml in the following way:

```
[project.optional-dependencies]
cpu = [
  "torch==2.2.2",
]
cu118 = [
  "torch==2.2.2",
]

[tool.uv]
required-version = "0.5.21"
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu118" },
  ],
]
package = true
python-preference = "only-managed"

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
  { index = "pytorch-cu118", extra = "cu118", marker = "platform_system != 'Darwin'" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu118"
url = "https://download.pytorch.org/whl/cu118"
explicit = true
```

 I'm  on 
``` import platform;  platform.freedesktop_os_release()
{'NAME': 'Debian GNU/Linux', 'ID': 'debian', 'PRETTY_NAME': 'Debian GNU/Linux 11 (bullseye)', 'VERSION_ID': '11', 'VERSION': '11 (bullseye)', 'VERSION_CODENAME': 'bullseye', 'HOME_URL': 'https://www.debian.org/', 'SUPPORT_URL': 'https://www.debian.org/support', 'BUG_REPORT_URL': 'https://bugs.debian.org/'
```
 machine

If I do a fresh installation without having a lock file locally by running `uv sync --extra=cpu`, it fails with:

```
error: Distribution `torch==2.2.2 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

---

_Renamed from "UV (0.5.21) fails to install pytorch" to "uv 0.5.21 fails to install pytorch" by @kasperipalkama on 2025-01-20 09:32_

---

_Comment by @charliermarsh on 2025-01-20 14:25_

What is your Python version? Are you using Python 3.13?

---

_Label `question` added by @charliermarsh on 2025-01-20 14:25_

---

_Comment by @kasperipalkama on 2025-01-20 14:30_

> What is your Python version? Are you using Python 3.13?

I'm using 3.10.12:
```
% uv run python --version
Python 3.10.12
```

---

_Label `bug` added by @charliermarsh on 2025-01-20 14:33_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-20 14:33_

---

_Comment by @charliermarsh on 2025-01-20 14:34_

This was a regression. You can use 0.5.20 for the time being if needed.

---

_Label `question` removed by @charliermarsh on 2025-01-20 14:35_

---

_Comment by @kasperipalkama on 2025-01-20 14:54_

When using 0.5.20, the fresh installation works. However, if I add any new dependency to pyproject.toml and run `uv sync --extra=cpu` to install it, it fails to install pytorch with the same error message . This is not the case at least in 0.5.14

---

_Comment by @charliermarsh on 2025-01-20 14:57_

Please provide a complete reproduction: the exact `pyproject.toml`, `uv.lock`, evidence of the Python and uv version, the specific command you're running, and the failure you're seeing.

---

_Comment by @kasperipalkama on 2025-01-20 15:16_

The uv lock file is converted to txt file because github does not allow pasting .lock files
[uv.txt from the fresh install ](https://github.com/user-attachments/files/18479288/uv.txt)

pyproject.toml at the beginnnig:

``` 
[project]
name = "ads-mega-model"
dynamic = ["version"]
requires-python = "==3.10.12"
dependencies = [
    "bokeh==3.4.1",
    "icecream==2.1.3",
    "tensorboard==2.17.0",
    "lightning==2.2.5",
    "pytorch-lightning==2.4.0",
    "watermark==2.4.3",
    "optuna==3.6.1",
    "omegaconf==2.3.0",
    "ray[data,train,tune,serve]==2.37.0",
    "gcsfs==2024.6.0",
    "pyarrow==15.0.0",
    "pandas==2.2.2",
    "fire==0.6.0",
    "google-cloud-core==2.4.1",
    "google-cloud-firestore==2.19.0",
    "google-cloud-storage==2.18.2",
    "google-cloud-aiplatform==1.70.0",
    "google-cloud-bigquery[pandas]==3.26.0",
    "google-cloud-secret-manager==2.22.0",
    "mlflow==2.14.3",
    "kfp==2.8.0",
    "pyproject-metadata==0.8.0",
    "joblib==1.4.2",
    "toml==0.10.2",
    "wandb==0.17.6",
    "torchestra==0.1.6",
    "tenacity==9.0.0",
    "tritonclient==2.41.1",
    "opentelemetry-sdk==1.27.0",  # latest version that works with ray 2.37.0
    "opentelemetry-exporter-otlp==1.27.0",  # latest version that works with ray 2.37.0
    "pip==24.3.1",
]

[dependency-groups]
dev = [
    "ruff==0.5.7",
    "mypy==1.10.0",
    "pytest==8.2.2",
    "pytest-xdist==3.6.1",
    "pytest-order==1.2.1",
    "pytest-cov==5.0.0",
    "pytest-mock==3.14.0",
    "editorconfig-checker==2.7.3",
    "pre-commit==2.12.1",
    "types-requests==2.31.0.6",
    "pandas-stubs==2.2.2.240603",
    "pyarrow-stubs==10.0.1.7",
]

[project.optional-dependencies]
cpu = [
  "torch==2.2.2",
]
cu118 = [
  "torch==2.2.2",
]

[tool.uv]
required-version = "0.5.20"
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu118" },
  ],
]
package = true
python-preference = "only-managed"

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
  { index = "pytorch-cu118", extra = "cu118", marker = "platform_system != 'Darwin'" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu118"
url = "https://download.pytorch.org/whl/cu118"
explicit = true


[tool.ruff]
line-length = 120
indent-width = 4
extend-exclude = ["**/*.ipynb", "**/*.md"]

[tool.ruff.format]
quote-style = "double"
indent-style = "space"
docstring-code-format = true
line-ending = "lf"

[tool.ruff.lint]
extend-select = ["I"]

[tool.mypy]
exclude = [
    "build",
]
mypy_path = "stubs"

[[tool.mypy.overrides]]
module="toml.*"
ignore_missing_imports = true

[[tool.mypy.overrides]]
module="fire.*"
ignore_missing_imports = true

[[tool.mypy.overrides]]
module="fsspec.*"
ignore_missing_imports = true

[[tool.mypy.overrides]]
module="google.*"
ignore_missing_imports = true

[[tool.mypy.overrides]]
module="joblib.*"
ignore_missing_imports = true

[[tool.mypy.overrides]]
module="kfp.*"
ignore_missing_imports = true

[[tool.mypy.overrides]]
module="kubernetes.*"
ignore_missing_imports = true

[[tool.mypy.overrides]]
module="mlflow.*"
ignore_missing_imports = true

[[tool.mypy.overrides]]
module="vertexai.*"
ignore_missing_imports = true

[[tool.mypy.overrides]]
module="vertex_ray.*"
ignore_missing_imports = true

[[tool.mypy.overrides]]
module="tritonclient.*"
ignore_missing_imports = true

[tool.coverage]
run.omit = [
  "*/.local/*",
  "__*__.py",
  "*/test_*.py",
]
run.source = ["src"]
run.relative_files = true
run.branch = true
report.skip_empty = true
# covege report for local development
html.directory = ".local/htmlcov"
# covege report for CI + Quality Gate
xml.output = ".local/coverage.xml"
```

python version: 

```
uv run --extra=cpu python --version
Python 3.10.12
```
uv version:

```
uv version
uv 0.5.20
```

After adding e.g. "numpy<2" to the dependencies and reinstalling them, it  fails to find distribution for torch:

```
uv sync --extra=cpu
Resolved 254 packages in 1.99s
error: Distribution `torch==2.2.2 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
```


---

_Comment by @charliermarsh on 2025-01-20 15:59_

Thanks.

---

_Comment by @charliermarsh on 2025-01-20 17:06_

I think the second thing is a different bug. I'll track it in a separate issue.

---

_Closed by @charliermarsh on 2025-01-20 17:23_

---

_Comment by @charliermarsh on 2025-01-20 17:23_

Tracking the second piece in: https://github.com/astral-sh/uv/issues/10783

---
