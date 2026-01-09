---
number: 5001
title: flexible install version seems to conflict with uv pip install
type: issue
state: closed
author: rbavery
labels:
  - needs-mre
assignees: []
created_at: 2024-07-12T01:10:32Z
updated_at: 2024-07-12T21:33:34Z
url: https://github.com/astral-sh/uv/issues/5001
synced_at: 2026-01-07T13:12:17-06:00
---

# flexible install version seems to conflict with uv pip install

---

_Issue opened by @rbavery on 2024-07-12 01:10_

I have a package that depends on s3fs. When I try to install the package with pip, everything works. But [uv handles prereleases differently ](https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#pre-release-compatibility) and I think this conflicts with [the flexible version identifier for fsspec](https://github.com/fsspec/s3fs/blob/main/requirements.txt#L2). It looks like uv is assuming that I want to install a prerelease version of fsspec.


```
 > [wherobots-inference-gpu  7/10] RUN uv pip install dockers/gpu/dist/wherobots_inference_gpu-1.3.0-py3-none-any.whl --extra-index-url https://download.pytorch.org/whl/cu121 --prerelease=allow && uv pip install pytest:                                                                                                                                                                                 
2.163   × No solution found when resolving dependencies:                                                                                                                                              
2.163   ╰─▶ Because only the following versions of fsspec are available:
2.163           fsspec<2024.6.1.dev0
2.163           fsspec>=2024.6.2.dev0
2.163       and s3fs==2024.6.1 depends on fsspec>=2024.6.1.dev0,<2024.6.2.dev0, we
2.163       can conclude that s3fs==2024.6.1 cannot be used. (1)
2.163 
2.163       Because only the following versions of s3fs are available:
2.163           s3fs<=2024.3.1
2.163           s3fs==2024.5.0
2.163           s3fs==2024.6.0
2.163           s3fs==2024.6.1
2.163           s3fs>=2025.0.0
2.163       and s3fs==2024.3.1 depends on fsspec==2024.3.1, we can conclude that
2.163       s3fs>=2024.3.1,<2024.5.0 depends on fsspec==2024.3.1.
2.163       And because there is no version of fsspec==2024.3.1, we can conclude
2.163       that s3fs>=2024.3.1,<2024.5.0 cannot be used.
2.163       And because s3fs==2024.5.0 depends on
2.163       fsspec>=2024.5.0.dev0,<2024.5.1.dev0 and only the following versions of
2.163       fsspec are available:
2.163           fsspec<2024.5.0.dev0
2.163           fsspec>=2024.5.1.dev0
2.163       we can conclude that s3fs>=2024.3.1,<2024.6.0 cannot be used.
2.163       And because s3fs==2024.6.0 depends on
2.163       fsspec>=2024.6.0.dev0,<2024.6.1.dev0 and only the following versions of
2.163       fsspec are available:
2.163           fsspec<2024.6.0.dev0
2.163           fsspec>=2024.6.1.dev0
2.163       we can conclude that s3fs>=2024.3.1,<2024.6.1 cannot be used.
2.163       And because we know from (1) that s3fs==2024.6.1 cannot be used, we can
2.163       conclude that s3fs>=2024.3.1 cannot be used.
2.163       And because wherobots-inference-gpu==1.3.0 depends on s3fs>=2024.3.1, we
2.163       can conclude that wherobots-inference-gpu==1.3.0 cannot be used.
2.163       And because only wherobots-inference-gpu==1.3.0 is available and you
2.163       require wherobots-inference-gpu, we can conclude that the requirements
2.163       are unsatisfiable.
```

if I don't allow prereleases, I get an error suggesting to allow prereleases

```
> [wherobots-inference-gpu  7/10] RUN uv pip install dockers/gpu/dist/wherobots_inference_gpu-1.3.0-py3-none-any.whl --extra-index-url https://download.pytorch.org/whl/cu121 && uv pip install pytest:        
2.115   × No solution found when resolving dependencies:                                                
2.115   ╰─▶ Because only the following versions of fsspec are available:                                
2.115           fsspec<2024.6.1.dev0                                                                    
2.115           fsspec>=2024.6.2.dev0
2.115       and s3fs==2024.6.1 depends on fsspec>=2024.6.1.dev0,<2024.6.2.dev0, we
2.115       can conclude that s3fs==2024.6.1 cannot be used. (1)
2.115 
2.115       Because only the following versions of s3fs are available:
2.115           s3fs<=2024.3.1
2.115           s3fs==2024.5.0
2.115           s3fs==2024.6.0
2.115           s3fs==2024.6.1
2.115           s3fs>=2025.0.0
2.115       and s3fs==2024.3.1 depends on fsspec==2024.3.1, we can conclude that
2.115       s3fs>=2024.3.1,<2024.5.0 depends on fsspec==2024.3.1.
2.115       And because there is no version of fsspec==2024.3.1, we can conclude
2.115       that s3fs>=2024.3.1,<2024.5.0 cannot be used.
2.115       And because s3fs==2024.5.0 depends on
2.115       fsspec>=2024.5.0.dev0,<2024.5.1.dev0 and only the following versions of
2.115       fsspec are available:
2.115           fsspec<2024.5.0.dev0
2.115           fsspec>=2024.5.1.dev0
2.115       we can conclude that s3fs>=2024.3.1,<2024.6.0 cannot be used.
2.115       And because s3fs==2024.6.0 depends on
2.115       fsspec>=2024.6.0.dev0,<2024.6.1.dev0 and only the following versions of
2.115       fsspec are available:
2.115           fsspec<2024.6.0.dev0
2.115           fsspec>=2024.6.1.dev0
2.115       we can conclude that s3fs>=2024.3.1,<2024.6.1 cannot be used.
2.115       And because we know from (1) that s3fs==2024.6.1 cannot be used, we can
2.115       conclude that s3fs>=2024.3.1 cannot be used.
2.115       And because wherobots-inference-gpu==1.3.0 depends on s3fs>=2024.3.1, we
2.115       can conclude that wherobots-inference-gpu==1.3.0 cannot be used.
2.115       And because only wherobots-inference-gpu==1.3.0 is available and you
2.115       require wherobots-inference-gpu, we can conclude that the requirements
2.115       are unsatisfiable.
2.115 
2.115       hint: fsspec was requested with a pre-release marker (e.g.,
2.115       fsspec>=2024.6.1.dev0,<2024.6.2.dev0), but pre-releases weren't enabled
2.115       (try: `--prerelease=allow`)
------
failed to solve: process "/bin/bash -o pipefail -c uv pip install dockers/gpu/dist/wherobots_inference_gpu-${WHEROBOTS_INF_VERSION}-py3-none-any.whl --extra-index-url https://download.pytorch.org/whl/cu121 && uv pip install pytest" did not complete successfully: exit code: 1

```

I'm not sure if this is a bug with uv or if this is an unsupported version identifier issue.

I'm running with the latest uv on ubuntu

---

_Comment by @charliermarsh on 2024-07-12 01:13_

Do you know what the requirements are for that wheel? E.g., this works fine: `echo "fsspec~=2024.6.1" | uv pip compile -`

---

_Label `needs-mre` added by @charliermarsh on 2024-07-12 02:36_

---

_Comment by @rbavery on 2024-07-12 20:11_

I'm having trouble making an MRE requirements.txt. The wheel is built with poetry, the toml is below

```

# Poetry pyproject.toml: https://python-poetry.org/docs/pyproject/
[build-system]
requires = ["poetry_core>=1.0.0"]
build-backend = "poetry.core.masonry.api"

[tool.poetry]
name = "wherobots-inference-gpu"
version = "1.3.1"
description = 'Train, test, and run your models and popular open source models on your geospatial data!'
authors = ["Ryan Avery <ryan@wherobots.com>"]
license = "AGPL-3.0"
readme = "README.md"
homepage = "https://github.com/wherobots/wherobots-inference"
repository = "https://github.com/wherobots/wherobots-inference"
documentation = "https://github.com/wherobots/wherobots-inference#readme"
keywords = [] # Add your keywords if any
packages = [
  {include = "wherobots", from = "../../src/"},
]

[tool.poetry.dependencies]
python = "^3.10"
boto3=">=1.33.60"
kornia = "^0.7.2"
geopandas = "0.14.4"
memory-profiler = "^0.61.0"
numpy = "^1.26.3"
pandas="1.5.3"
pyproj = "^3.6.1"
pystac = "^1.9.0"
pystac-client = "^0.7.5"
rasterio = ">=1.2.10"
s3fs = "2024.6.1" # https://github.com/conda/conda/issues/13619
scikit-image = "^0.22.0"
shapely = ">=2.0.0"
stac-model = "0.2"
tifffile = "^2023.12.9"
pyarrow = "^15.0.0"
pyopenssl = "^24.0.0" # https://github.com/conda/conda/issues/13619
matplotlib = "^3.8.3"
torch = {version = "2.3.0+cu121", source = "pytorch-gpu", markers = "python_version == '3.10' and sys_platform == 'linux'"}
torchvision = {version = "0.18.0+cu121", source = "pytorch-gpu", markers = "python_version == '3.10' and sys_platform == 'linux'"}
# pytorch-triton = {version = "3.0.0", source = "pytorch-gpu", markers = "python_version == '3.10' and sys_platform == 'linux'"}
ray = {extras = ["data"], version = "^2.12.0"}
wandb = "^0.17.3"

[[tool.poetry.source]]
name = "pytorch-gpu-nightly"
url = "https://download.pytorch.org/whl/nightly/cu121"
priority = "explicit"

[[tool.poetry.source]]
name = "pytorch-gpu"
url = "https://download.pytorch.org/whl/cu121"
priority = "explicit"
```

but when I export that to a requirements.txt without hashes, I get a different error when installing the requirements.txt

```
# rave at rave-desktop in ~/work/wherobots-inference/dockers/gpu on git:byom ✖︎ [13:09:28]
→ uv pip install -r requirements.txt                                                                                                            
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of certifi{python_version >= '3.10' and python_version < '4.0'}==2024.7.4 and you require certifi{python_version >= '3.10' and python_version < '4.0'}==2024.7.4, we can conclude
      that the requirements are unsatisfiable.
```

even though [that version of certifi is on pypi](https://pypi.org/project/certifi/)

```
--extra-index-url https://download.pytorch.org/whl/cu121

affine==2.4.0 ; python_version >= "3.10" and python_version < "4.0"
aiobotocore==2.13.1 ; python_version >= "3.10" and python_version < "4.0"
aiohttp==3.9.5 ; python_version >= "3.10" and python_version < "4.0"
aioitertools==0.11.0 ; python_version >= "3.10" and python_version < "4.0"
aiosignal==1.3.1 ; python_version >= "3.10" and python_version < "4.0"
annotated-types==0.7.0 ; python_version >= "3.10" and python_version < "4.0"
async-timeout==4.0.3 ; python_version >= "3.10" and python_version < "3.11"
attrs==23.2.0 ; python_version >= "3.10" and python_version < "4.0"
boto3==1.34.131 ; python_version >= "3.10" and python_version < "4.0"
botocore==1.34.131 ; python_version >= "3.10" and python_version < "4.0"
certifi==2024.7.4 ; python_version >= "3.10" and python_version < "4.0"
cffi==1.16.0 ; python_version >= "3.10" and python_version < "4.0" and platform_python_implementation != "PyPy"
charset-normalizer==3.3.2 ; python_version >= "3.10" and python_version < "4.0"
click-plugins==1.1.1 ; python_version >= "3.10" and python_version < "4.0"
click==8.1.7 ; python_version >= "3.10" and python_version < "4.0"
cligj==0.7.2 ; python_version >= "3.10" and python_version < "4"
colorama==0.4.6 ; python_version >= "3.10" and python_version < "4.0"
contourpy==1.2.1 ; python_version >= "3.10" and python_version < "4.0"
cryptography==42.0.8 ; python_version >= "3.10" and python_version < "4.0"
cycler==0.12.1 ; python_version >= "3.10" and python_version < "4.0"
docker-pycreds==0.4.0 ; python_version >= "3.10" and python_version < "4.0"
filelock==3.15.4 ; python_version >= "3.10" and python_version < "4.0"
fiona==1.9.6 ; python_version >= "3.10" and python_version < "4.0"
fonttools==4.53.1 ; python_version >= "3.10" and python_version < "4.0"
frozenlist==1.4.1 ; python_version >= "3.10" and python_version < "4.0"
fsspec==2024.6.1 ; python_version >= "3.10" and python_version < "4.0"
geopandas==0.14.4 ; python_version >= "3.10" and python_version < "4.0"
gitdb==4.0.11 ; python_version >= "3.10" and python_version < "4.0"
gitpython==3.1.43 ; python_version >= "3.10" and python_version < "4.0"
idna==3.7 ; python_version >= "3.10" and python_version < "4.0"
imageio==2.34.2 ; python_version >= "3.10" and python_version < "4.0"
intel-openmp==2021.4.0 ; python_version >= "3.10" and python_version < "4.0" and platform_system == "Windows"
jinja2==3.1.4 ; python_version >= "3.10" and python_version < "4.0"
jmespath==1.0.1 ; python_version >= "3.10" and python_version < "4.0"
jsonschema-specifications==2023.12.1 ; python_version >= "3.10" and python_version < "4.0"
jsonschema==4.23.0 ; python_version >= "3.10" and python_version < "4.0"
kiwisolver==1.4.5 ; python_version >= "3.10" and python_version < "4.0"
kornia-rs==0.1.5 ; python_version >= "3.10" and python_version < "4.0"
kornia==0.7.3 ; python_version >= "3.10" and python_version < "4.0"
lazy-loader==0.4 ; python_version >= "3.10" and python_version < "4.0"
markdown-it-py==3.0.0 ; python_version >= "3.10" and python_version < "4.0"
markupsafe==2.1.5 ; python_version >= "3.10" and python_version < "4.0"
matplotlib==3.9.1 ; python_version >= "3.10" and python_version < "4.0"
mdurl==0.1.2 ; python_version >= "3.10" and python_version < "4.0"
memory-profiler==0.61.0 ; python_version >= "3.10" and python_version < "4.0"
mkl==2021.4.0 ; python_version >= "3.10" and python_version < "4.0" and platform_system == "Windows"
mpmath==1.3.0 ; python_version >= "3.10" and python_version < "4.0"
msgpack==1.0.8 ; python_version >= "3.10" and python_version < "4.0"
multidict==6.0.5 ; python_version >= "3.10" and python_version < "4.0"
networkx==3.3 ; python_version >= "3.10" and python_version < "4.0"
numpy==1.26.4 ; python_version >= "3.10" and python_version < "4.0"
nvidia-cublas-cu12==12.1.3.1 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version >= "3.10" and python_version < "4.0"
nvidia-cuda-cupti-cu12==12.1.105 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version >= "3.10" and python_version < "4.0"
nvidia-cuda-nvrtc-cu12==12.1.105 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version >= "3.10" and python_version < "4.0"
nvidia-cuda-runtime-cu12==12.1.105 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version >= "3.10" and python_version < "4.0"
nvidia-cudnn-cu12==8.9.2.26 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version >= "3.10" and python_version < "4.0"
nvidia-cufft-cu12==11.0.2.54 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version >= "3.10" and python_version < "4.0"
nvidia-curand-cu12==10.3.2.106 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version >= "3.10" and python_version < "4.0"
nvidia-cusolver-cu12==11.4.5.107 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version >= "3.10" and python_version < "4.0"
nvidia-cusparse-cu12==12.1.0.106 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version >= "3.10" and python_version < "4.0"
nvidia-nccl-cu12==2.20.5 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version >= "3.10" and python_version < "4.0"
nvidia-nvjitlink-cu12==12.5.82 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version >= "3.10" and python_version < "4.0"
nvidia-nvtx-cu12==12.1.105 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version >= "3.10" and python_version < "4.0"
packaging==24.1 ; python_version >= "3.10" and python_version < "4.0"
pandas==1.5.3 ; python_version >= "3.10" and python_version < "4.0"
pillow==10.4.0 ; python_version >= "3.10" and python_version < "4.0"
platformdirs==4.2.2 ; python_version >= "3.10" and python_version < "4.0"
protobuf==5.27.2 ; python_version >= "3.10" and python_version < "4.0"
psutil==6.0.0 ; python_version >= "3.10" and python_version < "4.0"
pyarrow==15.0.2 ; python_version >= "3.10" and python_version < "4.0"
pycparser==2.22 ; python_version >= "3.10" and python_version < "4.0" and platform_python_implementation != "PyPy"
pydantic-core==2.20.1 ; python_version >= "3.10" and python_version < "4.0"
pydantic==2.8.2 ; python_version >= "3.10" and python_version < "4.0"
pygments==2.18.0 ; python_version >= "3.10" and python_version < "4.0"
pyopenssl==24.1.0 ; python_version >= "3.10" and python_version < "4.0"
pyparsing==3.1.2 ; python_version >= "3.10" and python_version < "4.0"
pyproj==3.6.1 ; python_version >= "3.10" and python_version < "4.0"
pystac-client==0.7.7 ; python_version >= "3.10" and python_version < "4.0"
pystac==1.10.1 ; python_version >= "3.10" and python_version < "4.0"
pystac[validation]==1.10.1 ; python_version >= "3.10" and python_version < "4.0"
python-dateutil==2.9.0.post0 ; python_version >= "3.10" and python_version < "4.0"
pytz==2024.1 ; python_version >= "3.10" and python_version < "4.0"
pyyaml==6.0.1 ; python_version >= "3.10" and python_version < "4.0"
rasterio==1.3.10 ; python_version >= "3.10" and python_version < "4.0"
ray[data]==2.32.0 ; python_version >= "3.10" and python_version < "4.0"
referencing==0.35.1 ; python_version >= "3.10" and python_version < "4.0"
requests==2.32.3 ; python_version >= "3.10" and python_version < "4.0"
rich==13.7.1 ; python_version >= "3.10" and python_version < "4.0"
rpds-py==0.19.0 ; python_version >= "3.10" and python_version < "4.0"
s3fs==2024.6.1 ; python_version >= "3.10" and python_version < "4.0"
s3transfer==0.10.2 ; python_version >= "3.10" and python_version < "4.0"
scikit-image==0.22.0 ; python_version >= "3.10" and python_version < "4.0"
scipy==1.14.0 ; python_version >= "3.10" and python_version < "4.0"
sentry-sdk==2.9.0 ; python_version >= "3.10" and python_version < "4.0"
setproctitle==1.3.3 ; python_version >= "3.10" and python_version < "4.0"
setuptools==70.3.0 ; python_version >= "3.10" and python_version < "4.0"
shapely==2.0.4 ; python_version >= "3.10" and python_version < "4.0"
shellingham==1.5.4 ; python_version >= "3.10" and python_version < "4.0"
six==1.16.0 ; python_version >= "3.10" and python_version < "4.0"
smmap==5.0.1 ; python_version >= "3.10" and python_version < "4.0"
snuggs==1.4.7 ; python_version >= "3.10" and python_version < "4.0"
stac-model==0.2.0 ; python_version >= "3.10" and python_version < "4.0"
sympy==1.13.0 ; python_version >= "3.10" and python_version < "4.0"
tbb==2021.13.0 ; python_version >= "3.10" and python_version < "4.0" and platform_system == "Windows"
tifffile==2023.12.9 ; python_version >= "3.10" and python_version < "4.0"
torch==2.3.0+cu121 ; python_version >= "3.10" and python_version < "4.0"
torchvision==0.18.0+cu121 ; python_version == "3.10" and sys_platform == "linux"
triton==2.3.0 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version < "3.12" and python_version >= "3.10"
typer[all]==0.9.4 ; python_version >= "3.10" and python_version < "4.0"
typing-extensions==4.12.2 ; python_version >= "3.10" and python_version < "4.0"
urllib3==2.2.2 ; python_version >= "3.10" and python_version < "4.0"
wandb==0.17.4 ; python_version >= "3.10" and python_version < "4.0"
wrapt==1.16.0 ; python_version >= "3.10" and python_version < "4.0"
yarl==1.9.4 ; python_version >= "3.10" and python_version < "4.0"
```






---

_Comment by @rbavery on 2024-07-12 20:49_

I've narrowed down the original issue to the following toml that only specifies the s3fs dependency

```
# Poetry pyproject.toml: https://python-poetry.org/docs/pyproject/
[build-system]
requires = ["poetry_core>=1.0.0"]
build-backend = "poetry.core.masonry.api"

[tool.poetry]
name = "wherobots-inference-gpu"
version = "1.3.1"
description = 'Train, test, and run your models and popular open source models on your geospatial data!'
authors = ["Ryan Avery <ryan@wherobots.com>"]
license = "AGPL-3.0"
readme = "README.md"
homepage = "https://github.com/wherobots/wherobots-inference"
repository = "https://github.com/wherobots/wherobots-inference"
documentation = "https://github.com/wherobots/wherobots-inference#readme"
keywords = [] # Add your keywords if any
packages = [
  {include = "wherobots", from = "../../src/"},
]

[tool.poetry.dependencies]
python = "^3.10"
s3fs = "2024.6.1" # https://github.com/conda/conda/issues/13619
```

It seems that this issue also occurs with the pystac-client dependency, and only when extra-index-url  is specified. The issue doesn't happen if the index url is not specified

```
# rave at rave-desktop in ~/work/wherobots-inference/dockers/gpu on git:byom ✖︎ [13:45:01]
→ uv pip install dist/wherobots_inference_gpu-1.3.1-py3-none-any.whl --extra-index-url https://download.pytorch.org/whl/cu121
  × No solution found when resolving dependencies:
  ╰─▶ Because only requests<2.28.2 is available and pystac-client>=0.7.5,<=0.7.7 depends on requests>=2.28.2, we can conclude that pystac-client>=0.7.5,<=0.7.7 cannot be used.
      And because only the following versions of pystac-client are available:
          pystac-client<=0.7.5
          pystac-client==0.7.6
          pystac-client==0.7.7
          pystac-client>=0.8.0
      and wherobots-inference-gpu==1.3.1 depends on pystac-client>=0.7.5,<0.8.0, we can conclude that wherobots-inference-gpu==1.3.1 cannot be used.
      And because only wherobots-inference-gpu==1.3.1 is available and you require wherobots-inference-gpu, we can conclude that the requirements are unsatisfiable.
```

Setup

```
uv venv                                                           
source .venv/bin/activate
poetry build-project
uv pip install dist/wherobots_inference_gpu-1.3.1-py3-none-any.whl --extra-index-url https://download.pytorch.org/whl/cu121 --prerelease=allow
```

installing the wheel with extra index url and prerelease=allow fails
```
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of fsspec are available:
          fsspec<2024.6.1.dev0
          fsspec>=2024.6.2.dev0
      and s3fs==2024.6.1 depends on fsspec>=2024.6.1.dev0,<2024.6.2.dev0, we can conclude that s3fs==2024.6.1 cannot be used.
      And because wherobots-inference-gpu==1.3.1 depends on s3fs==2024.6.1, we can conclude that wherobots-inference-gpu==1.3.1 cannot be used.
      And because only wherobots-inference-gpu==1.3.1 is available and you require wherobots-inference-gpu, we can conclude that the requirements are unsatisfiable.
```

installing the wheel with extra index url fails and suggests setting prerelease=allow
```
uv pip install dist/wherobots_inference_gpu-1.3.1-py3-none-any.whl --extra-index-url https://download.pytorch.org/whl/cu121                   
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of fsspec are available:
          fsspec<2024.6.1.dev0
          fsspec>=2024.6.2.dev0
      and s3fs==2024.6.1 depends on fsspec>=2024.6.1.dev0,<2024.6.2.dev0, we can conclude that s3fs==2024.6.1 cannot be used.
      And because wherobots-inference-gpu==1.3.1 depends on s3fs==2024.6.1, we can conclude that wherobots-inference-gpu==1.3.1 cannot be used.
      And because only wherobots-inference-gpu==1.3.1 is available and you require wherobots-inference-gpu, we can conclude that the requirements are unsatisfiable.

      hint: fsspec was requested with a pre-release marker (e.g., fsspec>=2024.6.1.dev0,<2024.6.2.dev0), but pre-releases weren't enabled (try: `--prerelease=allow`)
```

installing the wheel without extra index url works
```
uv pip install dist/wherobots_inference_gpu-1.3.1-py3-none-any.whl                                                         
Resolved 28 packages in 130ms
Prepared 1 package in 1ms
Installed 28 packages in 47ms
 + aiobotocore==2.13.1
 + aiohttp==3.9.5
 + aioitertools==0.11.0
 + aiosignal==1.3.1
 + async-timeout==4.0.3
 + attrs==23.2.0
 + botocore==1.34.131
 + certifi==2024.7.4
 + charset-normalizer==3.3.2
 + frozenlist==1.4.1
 + fsspec==2024.6.1
 + idna==3.7
 + jmespath==1.0.1
 + jsonschema==4.23.0
 + jsonschema-specifications==2023.12.1
 + multidict==6.0.5
 + pystac==1.10.1
 + pystac-client==0.7.7
 + python-dateutil==2.9.0.post0
 + referencing==0.35.1
 + requests==2.32.3
 + rpds-py==0.19.0
 + s3fs==2024.6.1
 + six==1.16.0
 + urllib3==2.2.2
 + wherobots-inference-gpu==1.3.1 (from file:///home/rave/work/wherobots-inference/dockers/gpu/dist/wherobots_inference_gpu-1.3.1-py3-none-any.whl)
 + wrapt==1.16.0
 + yarl==1.9.4
```


If I export that toml to a requirements txt like so, I can install it if I don't specify the index url, even though it is in the requirements.txt. But if I specify the index url as a command line arg, it fails with the certifi issue again.

```
# rave at rave-desktop in ~/work/wherobots-inference/dockers/gpu on git:byom ✖︎ [13:39:15]
→ uv pip install -r requirements.txt --extra-index-url https://download.pytorch.org/whl/cu121
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of certifi{python_version >= '3.10' and python_version < '4.0'}==2024.7.4 and you require certifi{python_version >= '3.10' and python_version < '4.0'}==2024.7.4, we can conclude
      that the requirements are unsatisfiable.

# rave at rave-desktop in ~/work/wherobots-inference/dockers/gpu on git:byom ✖︎ [13:39:35]
→ uv pip install -r requirements.txt                                                         
Resolved 27 packages in 78ms
Installed 27 packages in 48ms
 + aiobotocore==2.13.1
 + aiohttp==3.9.5
 + aioitertools==0.11.0
 + aiosignal==1.3.1
 + async-timeout==4.0.3
 + attrs==23.2.0
 + botocore==1.34.131
 + certifi==2024.7.4
 + charset-normalizer==3.3.2
 + frozenlist==1.4.1
 + fsspec==2024.6.1
 + idna==3.7
 + jmespath==1.0.1
 + jsonschema==4.23.0
 + jsonschema-specifications==2023.12.1
 + multidict==6.0.5
 + pystac==1.10.1
 + pystac-client==0.7.7
 + python-dateutil==2.9.0.post0
 + referencing==0.35.1
 + requests==2.32.3
 + rpds-py==0.19.0
 + s3fs==2024.6.1
 + six==1.16.0
 + urllib3==2.2.2
 + wrapt==1.16.0
 + yarl==1.9.4
```

The requirements.txt that produces the certifi issue is here, maybe that could be an MRE that also represents the issue witht he toml, I'm not sure if they are the same issue.

```
attrs==23.2.0 ; python_version >= "3.10" and python_version < "4.0"
certifi==2024.7.4 ; python_version >= "3.10" and python_version < "4.0"
charset-normalizer==3.3.2 ; python_version >= "3.10" and python_version < "4.0"
idna==3.7 ; python_version >= "3.10" and python_version < "4.0"
jsonschema-specifications==2023.12.1 ; python_version >= "3.10" and python_version < "4.0"
jsonschema==4.23.0 ; python_version >= "3.10" and python_version < "4.0"
pystac-client==0.7.7 ; python_version >= "3.10" and python_version < "4.0"
pystac[validation]==1.10.1 ; python_version >= "3.10" and python_version < "4.0"
python-dateutil==2.9.0.post0 ; python_version >= "3.10" and python_version < "4.0"
referencing==0.35.1 ; python_version >= "3.10" and python_version < "4.0"
requests==2.32.3 ; python_version >= "3.10" and python_version < "4.0"
rpds-py==0.19.0 ; python_version >= "3.10" and python_version < "4.0"
six==1.16.0 ; python_version >= "3.10" and python_version < "4.0"
urllib3==2.2.2 ; python_version >= "3.10" and python_version < "4.0"

```

---

_Comment by @notatallshaw on 2024-07-12 20:52_

Sounds like an index-strategy issue, I would reccomend reading: https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#packages-that-exist-on-multiple-indexes

You may need to use `--index-strategy unsafe-first-match` or `--index-strategy unsafe-best-match` , please read the details though, "unsafe" is in the name for a reason.

---

_Comment by @rbavery on 2024-07-12 20:56_

Thanks for the tip! Are you saying that the issue is that `certifi` and `s3fs` are on the extra index I specified and have different version constraints? I thought only torch dependencies would be on that index. so I would expect the default first-match strategy to still behave the same for certifi and s3fs even if the extra-index-url is specified.

`index-strategy unsafe-best-match` works, thanks a bunch.

---

_Comment by @notatallshaw on 2024-07-12 21:01_

> Are you saying that the issue is that `certifi` and `s3fs` are on the extra index I specified and have different version constraints?

You can check packages on that index yourself looking directly as the Simple API: https://download.pytorch.org/whl/cu121

I'm not too familiar with pytorch, but I think they have historical reasons to put dependency packages on their own index.

There are proposed solutions (in particular PEP 708) but they're not implemented yet, and uv's default behavior protect against real world attacks: https://pytorch.org/blog/compromised-nightly-dependency/

---

_Comment by @charliermarsh on 2024-07-12 21:02_

I do see `ceritfi` on there at least... Anyway, glad this could be resolved and sorry for the trouble! Thanks too to @notatallshaw.

---

_Closed by @charliermarsh on 2024-07-12 21:02_

---

_Comment by @rbavery on 2024-07-12 21:33_

ah thanks so much for explaining @notatallshaw , didn't know I could check the packages in that index.

---
