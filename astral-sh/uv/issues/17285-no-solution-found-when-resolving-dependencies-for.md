---
number: 17285
title: No solution found when resolving dependencies for split
type: issue
state: closed
author: aswordok
labels:
  - question
assignees: []
created_at: 2026-01-02T13:28:25Z
updated_at: 2026-01-03T04:20:07Z
url: https://github.com/astral-sh/uv/issues/17285
synced_at: 2026-01-10T01:26:16Z
---

# No solution found when resolving dependencies for split

---

_Issue opened by @aswordok on 2026-01-02 13:28_

### Question

```
D:\+AI\uv\313>uv add dist\llama_cpp_python-0.3.19-cp313-cp313-win_amd64.whl
  x No solution found when resolving dependencies for split (markers: python_full_version >= '3.14' and sys_platform
  | == 'win32'):
  `-> Because llama-cpp-python==0.3.19 has no wheels with a matching Python version tag (e.g., `cp313`) and only
      llama-cpp-python==0.3.19 is available, we can conclude that all versions of llama-cpp-python cannot be used.
      And because your project depends on llama-cpp-python, we can conclude that your project's requirements are
      unsatisfiable.

      hint: While the active Python version is 3.13, the resolution failed for other Python versions supported by your
      project. Consider limiting your project's supported Python versions using `requires-python`.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip
        locking and syncing.

D:\+AI\uv\313>type pyproject.toml
[project]
name = "313"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "accelerate>=1.2.1",
    "addict>=2.4.0",
    "aggdraw>=1.3.19",
    "aiohttp>=3.11.8",
    "albumentations>=1.4.10",
    "alembic>=1.17.0",
    "annotated-doc>=0.0.4",
    "anyio>=4.11.0",
    "audioop-lts>=0.2.2",
    "av>=14.2.0",
    "backoff>=2.2.1",
    "beautifulsoup4>=4.14.2",
    "bitsandbytes>=0.48.2",
    "blend-modes>=2.2.0",
    "blind-watermark>=0.4.4",
    "build>=1.3.0",
    "cache-dit>=1.1.4",
    "cachetools>=6.2.2",
    "chardet>=5.2.0",
    "click>=8.1.8",
    "clip-interrogator>=0.6.0",
    "cmake>=4.2.0",
    "color-matcher>=0.6.0",
    "colorlog>=6.10.1",
    "colour-science>=0.4.6",
    "comfyui-embedded-docs==0.3.1",
    "comfyui-frontend-package==1.35.9",
    "comfyui-workflow-templates==0.7.64",
    "cssselect2>=0.8.0",
    "cstr",
    "dashscope>=1.25.2",
    "dataclasses-json>=0.6.7",
    "decord>=0.6.0",
    "deepdiff>=8.6.1",
    "defusedxml>=0.7.1",
    "diffusers>=0.33.0",
    "dill>=0.4.0",
    "diskcache>=5.6.3",
    "distro>=1.9.0",
    "docker>=7.1.0",
    "dynamicprompts==0.30.2",
    "editables>=0.5",
    "einops>=0.6.0",
    "facexlib>=0.3.0",
    "fairscale==0.4.0",
    "fastapi>=0.122.0",
    "ffmpeg>=1.4",
    "ffmpy",
    "flash-attn==2.8.3",
    "flet==0.27.6",
    "freetype-py>=2.5.1",
    "ftfy==6.1.1",
    "fvcore>=0.1.5.post20221221",
    "gdown>=5.2.0",
    "gguf>=0.17.1",
    "gitpython>=3.1.45",
    "google-ai-generativelanguage>=0.6.15",
    "google-api-core>=2.25.2",
    "google-api-python-client>=2.187.0",
    "google-auth>=2.43.0",
    "google-auth-httplib2>=0.2.1",
    "google-generativeai>=0.8.5",
    "googleapis-common-protos>=1.72.0",
    "grpcio-status>=1.71.2",
    "hatchling>=1.28.0",
    "hf-xet>=1.1.10",
    "httpcore>=1.0.9",
    "httplib2>=0.31.0",
    "httpx>=0.28.1",
    "huggingface-hub>=0.34",
    "imagehash>=4.3.2",
    "imageio>=2.37.0",
    "imageio-ffmpeg>=0.5.1",
    "img2texture",
    "insightface>=0.7.3",
    "iopath>=0.1.10",
    "jiter>=0.12.0",
    "joblib>=1.5.2",
    "kornia>=0.7.1",
    "lark>=1.3.0",
    "librosa==0.10.2",
    "lmdb>=1.4.1",
    "loguru>=0.7.3",
    "lxml>=6.0.2",
    "manifold3d>=3.3.2",
    "mapbox-earcut>=2.0.0",
    "marshmallow>=3.26.1",
    "matplotlib>=3.10.7",
    "matrix-nio>=0.25.2",
    "modelscope>=1.31.0",
    "moviepy>=2.2.1",
    "mss>=10.1.0",
    "mypy-extensions>=1.1.0",
    "ninja>=1.13.0",
    "numba>=0.62.1",
    "numpy>=1.25.0",
    "nunchaku",
    "nvidia-ml-py>=12.575.51",
    "omegaconf>=2.3.0",
    "onnxruntime>=1.23.2",
    "onnxruntime-gpu>=1.23.2",
    "open-clip-torch>=2.24.0",
    "openai>=1.12.0",
    "opencv-contrib-python>=4.11.0.86",
    "opencv-contrib-python-headless>=4.11.0.86",
    "opencv-python>=4.11.0.86",
    "opencv-python-headless>=4.11.0.86",
    "openunmix>=1.3.0",
    "orderly-set>=5.5.0",
    "packaging>=25.0",
    "pandas>=2.3.3",
    "pathlib>=1.0.1",
    "peft>=0.17.0",
    "piexif>=1.1.3",
    "pilgram>=2.0.0",
    "pillow>=10.3.0",
    "proglog>=0.1.12",
    "proto-plus>=1.26.1",
    "protobuf<6.0.0.dev0",
    "psd-tools>=1.12.0",
    "psutil>=5.9.0",
    "py-cpuinfo>=9.0.0",
    "pyasn1>=0.6.1",
    "pyasn1-modules>=0.4.2",
    "pybase64>=1.0.2",
    "pybind11>=3.0.1",
    "pycairo>=1.29.0",
    "pycocotools>=2.0.6",
    "pycollada>=0.9.2",
    "pydantic~=2.0",
    "pydantic-settings~=2.0",
    "pygithub>=2.8.1",
    "pyloudnorm>=0.1.1",
    "pymatting>=1.1.14",
    "pynvml>=13.0.1",
    "pysocks>=1.7.1",
    "pytorch-lightning>=2.5.5",
    "pytz>=2025.2",
    "pywavelets>=1.9.0",
    "pyyaml>=6.0.3",
    "pyzbar>=0.1.9",
    "qrcode>=8.2",
    "regex>=2025.10.22",
    "rembg>=2.0.68",
    "reportlab>=4.4.5",
    "requests>=2.32.5",
    "rich>=13.9.4",
    "rlpycairo>=0.4.0",
    "rotary-embedding-torch>=0.8.9",
    "rsa>=4.9.1",
    "rtree>=1.4.1",
    "safetensors>=0.4.2",
    "sageattention",
    "sam-2",
    "scikit-image>=0.20.0",
    "scikit-learn>=1.7.2",
    "scipy>=1.11.4",
    "segment-anything>=1.0",
    "sentencepiece>=0.2.0",
    "sentry-sdk>=2.46.0",
    "setuptools>=80.9.0",
    "shapely>=2.1.2",
    "shellingham>=1.5.4",
    "sniffio>=1.3.1",
    "soupsieve>=2.8",
    "spandrel>=0.4.1",
    "sparse-sageattn",
    "sqlalchemy>=2.0.44",
    "standard-aifc>=3.13.0",
    "standard-chunk>=3.13.0",
    "standard-sunau>=3.13.0",
    "starlette>=0.50.0",
    "supervision>=0.27.0",
    "svg-path>=7.0",
    "svglib>=1.6.0",
    "tabulate>=0.9.0",
    "tdqm>=0.0.1",
    "tensorflow>=2.20.0",
    "tensorrt>=10.13.3.9",
    "termcolor>=3.1.0",
    "timm>=0.4.12",
    "tinycss2>=1.5.1",
    "tokenizers>=0.13.3",
    "toml>=0.10.2",
    "tomli>=2.3.0",
    "torch>=2.0.0",
    "torchaudio>=2.3.0",
    "torchcodec>=0.8.1",
    "torchscale>=0.3.0",
    "torchsde>=0.2.6",
    "torchvision>=0.15.0",
    "tqdm>=4.67.1",
    "transformers>=4.50.3",
    "transparent-background>=1.3.2",
    "trimesh>=4.10.0",
    "triton-windows<3.5",
    "typer>=0.16.0",
    "typer-config>=1.4.3",
    "typing-extensions>=4.15.0",
    "typing-inspect>=0.9.0",
    "tyro==0.8.5",
    "tzdata>=2025.2",
    "ultralytics>=8.3.162",
    "uritemplate>=4.2.0",
    "uv>=0.9.4",
    "vhacdx>=0.0.9",
    "wandb>=0.23.0",
    "webencodings>=0.5.1",
    "wget>=3.2",
    "wheel>=0.45.1",
    "wheel-stub>=0.4.2",
    "win32-setctime>=1.2.0",
    "xformers",
    "xxhash>=3.6.0",
    "yacs>=0.1.8",
    "yapf>=0.43.0",
    "yarl>=1.18.0",
]
[tool.uv.sources]
torch = [
  { index = "pytorch-cu130", marker = "sys_platform == 'win32'" },
]
torchvision = [
  { index = "pytorch-cu130", marker = "sys_platform == 'win32'" },
]
torchaudio = [
  { index = "pytorch-cu130", marker = "sys_platform == 'win32'" },
]
xformers = { path = "tmp/xformers/dist/xformers-0.0.33+00a7a5f0.d20251020-cp39-abi3-win_amd64.whl" }
sparse-sageattn = { path = "dist/sparse_sageattn-0.1.0-py3-none-any.whl" }
sam-2 = { git = "https://github.com/facebookresearch/sam2" }
img2texture = { git = "https://github.com/ltdrdata/img2texture.git" }
ffmpy = { git = "https://github.com/ltdrdata/ffmpy.git" }
cstr = { git = "https://github.com/ltdrdata/cstr" }
sageattention = { path = "tmp/SageAttention/dist/sageattention-2.2.0-cp313-cp313-win_amd64.whl" }
nunchaku = { path = "dist/nunchaku-1.1.0+torch2.9-cp313-cp313-win_amd64.whl" }

[[tool.uv.index]]
url = "https://aiinfra.pkgs.visualstudio.com/PublicPackages/_packaging/onnxruntime-cuda-13/pypi/simple/"
[[tool.uv.index]]
name = "pytorch-cu130"
url = "https://download.pytorch.org/whl/cu130"
explicit = true

[tool.uv.workspace]
members = [
    "tmp/cc_torch",
]
[tool.uv.extra-build-dependencies]
flash-attn = ["torch"]

D:\+AI\uv\313>
```

My project requires Python 3.13 or higher. Why is it showing me the message "markers: python_full_version >= '3.14' "? How to solve it?


### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @aswordok on 2026-01-02 13:28_

---

_Comment by @woodruffw on 2026-01-02 15:21_

Hi @aswordok, where did you get `llama-cpp-python==0.3.19` from? The latest version on PyPI is 0.3.16, so I'm unable to inspect the metadata in that newer version.

From a first look, I strongly suspect that that wheel contains a marker for `>= 3.14`, which Python 3.13 wouldn't satisfy. That means your requirements are incompatible with your Python runtime version; you either need to update your runtime version to 3.14 *or* modify your requirements to make them compatible.

---

_Comment by @aswordok on 2026-01-03 01:04_

> Hi [@aswordok](https://github.com/aswordok), where did you get `llama-cpp-python==0.3.19` from? The latest version on PyPI is 0.3.16, so I'm unable to inspect the metadata in that newer version.嗨，你从哪里得到 `llama-cpp-python==0.3.19` 的？PyPI 上的最新版本是 0.3.16，所以我无法检查那个较新版本中的元数据。
> 
> From a first look, I strongly suspect that that wheel contains a marker for `>= 3.14`, which Python 3.13 wouldn't satisfy. That means your requirements are incompatible with your Python runtime version; you either need to update your runtime version to 3.14 _or_ modify your requirements to make them compatible.初步看，我强烈怀疑那个轮子包含了一个 `>= 3.14` 的标记，而 Python 3.13 不会满足这个标记。这意味着你的需求与你的 Python 运行时版本不兼容；你需要要么将运行时版本更新到 3.14，要么修改你的需求使其兼容。

uv add https://github.com/JamePeng/llama-cpp-python/releases/download/v0.3.19-cu130-Basic-win-20260101/llama_cpp_python-0.3.19-cp313-cp313-win_amd64.whl

because this whl support Qwen3VL, So I need to install it.

---

_Comment by @aswordok on 2026-01-03 03:07_

https://github.com/JamePeng/llama-cpp-python/issues/43#issuecomment-3705776449
This problem has been resolved by the author. Anyway, this seems to be a bug. This is the process I follow when creating a project:
cd /d D:\+AI\uv
md 313 && cd 313
uv init -p 3.13

---

_Closed by @aswordok on 2026-01-03 04:20_

---
