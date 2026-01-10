---
number: 13932
title: How to setup different index for different packages
type: issue
state: closed
author: mikel-brostrom
labels:
  - question
assignees: []
created_at: 2025-06-09T19:18:17Z
updated_at: 2025-06-09T21:54:54Z
url: https://github.com/astral-sh/uv/issues/13932
synced_at: 2026-01-10T01:25:40Z
---

# How to setup different index for different packages

---

_Issue opened by @mikel-brostrom on 2025-06-09 19:18_

### Question

The error:

```
➜  boxmot git:(max-py) ✗ uv sync --all-extras --all-groups
Resolved 180 packages in 0.78ms
error: Distribution `markupsafe==3.0.2 @ registry+https://download.pytorch.org/whl/cu121` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're using CPython 3.11 (`cp311`), but `markupsafe` (v3.0.2) only has wheels with the following Python implementation tag: `cp313`
```

My pyproject.toml:

```bash
# =====================================================
# pyproject.toml for BoxMOT ← UV with GPU by default
# =====================================================

#######################
# Alternative Indexes #
#######################

[[tool.uv.index]]
name     = "torch-cpu"
url      = "https://download.pytorch.org/whl/cpu"
explicit = true


[[tool.uv.index]]
name     = "torch-gpu"
url      = "https://download.pytorch.org/whl/cu121"

##################################
# Pin torch/vision to *an* index #
##################################

[tool.uv.sources]
torch = [
  { index = "torch-cpu", marker = "sys_platform == 'darwin'" },
  { index = "torch-gpu", marker = "sys_platform == 'linux' or sys_platform == 'win32'" },
]
torchvision = [
  { index = "torch-cpu", marker = "sys_platform == 'darwin'" },
  { index = "torch-gpu", marker = "sys_platform == 'linux' or sys_platform == 'win32'" },
]


#########################
# UV-specific Overrides #
#########################

[[tool.uv.dependency-metadata]]
name          = "yolox"
version       = "0.3.0"
requires-dist = ["onnx>=1.17.0", "onnxsim<1.0.0,>=0.4.36"]

[tool.uv]
no-build-isolation-package = ["yolox"]
# only torch and torchvision fetches from index
index-strategy = "unsafe-any-match"


############
# Flake8   #
############

[tool.flake8]
max-line-length = 120
exclude         = [".tox", "*.egg", "build", "temp"]
select          = ["E", "W", "F"]
doctests        = true
verbose         = 2
format          = "pylint"
ignore          = ["E731", "F405", "E402", "W504", "W605", "E741"]

##############
# Build System #
##############

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

###########
# Project #
###########

[project]
name            = "boxmot"
version         = "13.0.5"
description     = "BoxMOT: pluggable SOTA tracking modules for segmentation, object detection and pose estimation models"
readme          = "README.md"
requires-python = ">=3.9,<=3.13"
license         = { text = "AGPL-3.0" }

authors = [
  { name = "Mikel Broström" },
]

classifiers = [
  "Development Status :: 4 - Beta",
  "Intended Audience :: Developers",
  "Intended Audience :: Education",
  "Intended Audience :: Science/Research",
  "License :: OSI Approved :: GNU Affero General Public License v3 or later (AGPLv3+)",
  "Programming Language :: Python :: 3",
  "Programming Language :: Python :: 3.9",
  "Programming Language :: Python :: 3.10",
  "Programming Language :: Python :: 3.11",
  "Programming Language :: Python :: 3.12",
  "Topic :: Software Development",
  "Topic :: Scientific/Engineering",
  "Topic :: Scientific/Engineering :: Artificial Intelligence",
  "Topic :: Scientific/Engineering :: Image Recognition",
  "Topic :: Scientific/Engineering :: Image Processing",
]

keywords = [
  "tracking",
  "tracking-by-detection",
  "machine-learning",
  "deep-learning",
  "vision",
  "ML",
  "DL",
  "AI",
  "YOLO",
]

dependencies = [
  "setuptools",
  "filterpy<2.0.0,>=1.4.5",
  "gdown<6.0.0,>=5.1.0",
  "lapx<1.0.0,>=0.5.5",
  "loguru<1.0.0,>=0.7.2",
  "numpy",
  "pyyaml<7.0.0,>=6.0.1",
  "regex<2025.0.0,>=2024.0.0",
  "yacs<1.0.0,>=0.1.8",
  "scikit-learn<2.0.0,>=1.3.0",
  "pandas<3.0.0,>=2.0.0",
  "opencv-python<5.0.0,>=4.7.0",
  "ftfy<7.0.0,>=6.1.3",
  "gitpython<4.0.0,>=3.1.42",
  "torch>=2.2.1,<3.0.0",
  "torchvision>=0.17.1,<1.0.0",
]

[project.scripts]
boxmot = "boxmot.engine.cli:main"

#####################
# Dependency Groups #
#####################

[dependency-groups]
test = [
  "pytest<9.0.0,>=8.0.2",
  "isort<6.0.0,>=5.13.2",
  "pytest-cov<7.0.0,>=6.0.0",
]

yolo = ["ultralytics"]

evolve = [
  "ray<3.0.0,>=2.35.0",
  "ray[tune]<3.0.0,>=2.35.0",
  "plotly<6.0.0,>=5.19.0",
  "joblib<2.0.0,>=1.3.2",
]

dev = ["ipykernel<7.0.0,>=6.29.5"]

yolox-build-deps = [
  "setuptools",
  "torch<3.0.0,>=2.2.1",
]

onnx = ["onnx>=1.16.1", "onnxruntime>=1.20.0"]
openvino = ["openvino-dev>=2023.3"]
tflite = [
  "onnx2tf>=1.18.0",
  "onnx>=1.16.1",
  "tensorflow==2.17.0",
  "tf_keras",
  "sng4onnx>=1.0.1",
  "onnx_graphsurgeon>=0.3.26",
  "onnxslim>=0.1.31",
  "onnxruntime",
  "flatbuffers>=23.5.26",
  "onnxsim==0.4.33",
  "psutil==5.9.5",
  "ml_dtypes==0.3.2",
  "ai_edge_litert>=1.2.0",
]

```

Why is uv looking at `https://download.pytorch.org/whl/cu121` for `markupsafe==3.0.2`? Who should I do in order for uv only to use the indexes for this specific packages

### Platform

masOS (Darwin 24.4.0 arm64)

### Version

uv 0.7.12 (dc3fd4647 2025-06-06)

---

_Label `question` added by @mikel-brostrom on 2025-06-09 19:18_

---

_Comment by @charliermarsh on 2025-06-09 19:20_

Add `explicit = true` to the PyTorch indexes, e.g.:

```toml
[[tool.uv.index]]
name     = "torch-gpu"
url      = "https://download.pytorch.org/whl/cu121"
explicit = true
```

---

_Comment by @mikel-brostrom on 2025-06-09 21:54_

> Add `explicit = true` to the PyTorch indexes, e.g.:
> 
> [[tool.uv.index]]
> name     = "torch-gpu"
> url      = "https://download.pytorch.org/whl/cu121"
> explicit = true

Omg, what a miss on my side. Thanks 😄 

---

_Closed by @mikel-brostrom on 2025-06-09 21:54_

---
