---
number: 6250
title: "Build of `tensorrt-cu12` fails with \"No module named pip\""
type: issue
state: closed
author: Kartstig
labels:
  - question
assignees: []
created_at: 2024-08-20T13:31:00Z
updated_at: 2024-08-20T18:52:58Z
url: https://github.com/astral-sh/uv/issues/6250
synced_at: 2026-01-07T13:12:17-06:00
---

# Build of `tensorrt-cu12` fails with "No module named pip"

---

_Issue opened by @Kartstig on 2024-08-20 13:31_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
uv: `0.2.33`
python: `3.9.19`

I have the following in my requirements.txt:
```
# Minimal pip requirement list for AMPL with CUDA-enabled GPUs. This includes only the packages directly imported by AMPL code, plus a few others commonly used during development. It requires CUDA-enable GPUs and nvidia packages to be installed first.
-i https://download.pytorch.org/whl/cu118
--extra-index-url https://pypi.python.org/simple
# **Note**: for LC developers, comment out line 3 pypi install and use/uncommenting line 5:
#--extra-index-url https://wci-repo.llnl.gov/repository/pypi-group/simple
-f https://storage.googleapis.com/jax-releases/jax_cuda_releases.html
-f https://data.dgl.ai/wheels/cu118/repo.html


absl-py==2.1.0
aiohttp==3.9.5
aiosignal==1.3.1
arrow==1.3.0
asttokens==2.4.1
astunparse==1.6.3
async-timeout==4.0.3
attrs==23.2.0
bokeh==3.4.1
bravado==11.0.3
bravado-core==6.1.1
cachetools==5.3.3
certifi==2024.2.2
charset-normalizer==3.3.2
cloudpickle==3.0.0
cmake==3.29.3
coloredlogs==15.0.1
comm==0.2.2
contourpy==1.2.1
cycler==0.12.1
debugpy==1.8.1
decorator==5.1.1
deepchem==2.7.1
dgl==1.1.2+cu118
dgllife==0.3.2
dill==0.3.8
exceptiongroup==1.2.1
executing==2.0.1
filelock==3.14.0
flatbuffers==24.3.25
fonttools==4.52.4
fqdn==1.5.1
frozenlist==1.4.1
fsspec==2024.5.0
future==1.0.0
gast==0.5.4
google-auth==2.29.0
google-auth-oauthlib==1.0.0
google-pasta==0.2.0
grpcio==1.64.0
h5py==3.11.0
humanfriendly==10.0
hyperopt==0.2.7
idna==3.7
importlib_metadata==7.1.0
importlib_resources==6.4.0
ipykernel==6.29.4
ipython==8.18.1
isoduration==20.11.0
jax==0.4.16
jaxlib
jedi==0.19.1
Jinja2==3.1.4
joblib==1.4.2
jsonpointer==2.4
jsonref==1.1.0
jsonschema==4.22.0
jsonschema-specifications==2023.12.1
jupyter_client==8.6.2
jupyter_core==5.7.2
keras==2.14.0
kiwisolver==1.4.5
libclang==18.1.1
lightning==2.2.5
lightning-utilities==0.11.2
lit==18.1.6
llvmlite==0.42.0
maestrowf==1.1.10
Markdown==3.6
markdown-it-py==3.0.0
MarkupSafe==2.1.5
matplotlib==3.9.0
matplotlib-inline==0.1.7
matplotlib-venn==0.11.10
mdurl==0.1.2
ml-dtypes==0.2.0
MolVS==0.1.1
monotonic==1.6
mordred==1.2.0
mpmath==1.3.0
msgpack==1.0.8
multidict==6.0.5
nest-asyncio==1.6.0
networkx==2.8.8
numba==0.59.1
numpy==1.24.4
nvidia-cublas-cu11
nvidia-cuda-cupti-cu11==11.8.87
nvidia-cuda-nvcc-cu11==11.8.89
nvidia-cuda-runtime-cu11==11.8.89
nvidia-cudnn-cu11==9.1.1.17
nvidia-cufft-cu11==10.9.0.58
nvidia-cusolver-cu11==11.4.1.48
nvidia-cusparse-cu11==11.7.5.86
oauthlib==3.2.2
opt-einsum==3.3.0
packaging==24.0
pandas==2.2.2
parso==0.8.4
pexpect==4.9.0
pillow==10.3.0
platformdirs==4.2.2
prompt_toolkit==3.0.45
protobuf==4.25.3
psutil==5.9.8
ptyprocess==0.7.0
pure-eval==0.2.2
py4j==0.10.9.7
pyarrow==16.1.0
pyasn1==0.6.0
pyasn1_modules==0.4.0
Pygments==2.18.0
pynndescent==0.5.12
pyparsing==3.1.2
python-dateutil==2.9.0.post0
pytorch-lightning==2.2.5
pytz==2024.1
PyYAML==5.4.1
pyzmq==26.0.3
rdkit==2023.9.6
referencing==0.35.1
requests==2.32.3
requests-oauthlib==2.0.0
rfc3339-validator==0.1.4
rfc3986-validator==0.1.1
rich==13.7.1
rpds-py==0.18.1
rsa==4.9
scikit-learn==1.2.2
scipy==1.8.1
seaborn==0.13.2
simplejson==3.19.2
six==1.16.0
stack-data==0.6.3
swagger-spec-validator==3.0.3
sympy==1.12.1
tabulate==0.9.0
tensorboard==2.14.1
tensorboard-data-server==0.7.2
tensorflow==2.14.0
tensorflow-estimator==2.14.0
tensorflow-io-gcs-filesystem==0.37.0
tensorrt==10.0.1
tensorrt-cu12==10.0.1
termcolor==2.4.0
threadpoolctl==3.5.0
torch==2.*
torch_geometric==2.5.3
torchmetrics==1.4.0.post0
tornado==6.4
tqdm==4.66.4
traitlets==5.14.3
triton==2.*
types-python-dateutil==2.9.0.20240316
typing_extensions==4.12.0
tzdata==2024.1
umap-learn==0.5.6
uri-template==1.3.0
urllib3==2.2.1
wcwidth==0.2.13
webcolors==1.13
Werkzeug==3.0.3
wrapt==1.14.1
xgboost==2.0.3
xyzservices==2024.4.0
yarl==1.9.4
zipp==3.19.0

```

running `uv pip install` leads to the following:
```
DEBUG Calling `setuptools.build_meta:__legacy__.build_wheel("/home/herman/.cache/uv/built-wheels-v3/index/dcd4cb247a731f5c/tensorrt-cu12/10.0.1/jOoT6AHZgjt1wIPlJCiVw/.tmpOG6WPI", {}, None)`
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: tensorrt-cu12==10.0.1
  Caused by: Failed to build: `tensorrt-cu12==10.0.1`
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:
running bdist_wheel
running build
running build_py
copying tensorrt/__init__.py -> build/lib/tensorrt
running egg_info
writing tensorrt_cu12.egg-info/PKG-INFO
writing dependency_links to tensorrt_cu12.egg-info/dependency_links.txt
writing requirements to tensorrt_cu12.egg-info/requires.txt
writing top-level names to tensorrt_cu12.egg-info/top_level.txt
reading manifest file 'tensorrt_cu12.egg-info/SOURCES.txt'
adding license file 'LICENSE.txt'
writing manifest file 'tensorrt_cu12.egg-info/SOURCES.txt'
installing to build/bdist.linux-x86_64/wheel
running install
--- stderr:
/home/herman/.cache/uv/builds-v0/.tmpg2iSa0/bin/python: No module named pip
/home/herman/.cache/uv/builds-v0/.tmpg2iSa0/bin/python: No module named pip
Traceback (most recent call last):
  File "<string>", line 11, in <module>
  File "/home/herman/.cache/uv/builds-v0/.tmpg2iSa0/lib/python3.9/site-packages/setuptools/build_meta.py", line 420, in build_wheel
    return self._build_with_temp_dir(
  File "/home/herman/.cache/uv/builds-v0/.tmpg2iSa0/lib/python3.9/site-packages/setuptools/build_meta.py", line 402, in _build_with_temp_dir
    self.run_setup()
  File "/home/herman/.cache/uv/builds-v0/.tmpg2iSa0/lib/python3.9/site-packages/setuptools/build_meta.py", line 502, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/home/herman/.cache/uv/builds-v0/.tmpg2iSa0/lib/python3.9/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 121, in <module>
  File "/home/herman/.cache/uv/builds-v0/.tmpg2iSa0/lib/python3.9/site-packages/setuptools/__init__.py", line 111, in setup
    return distutils.core.setup(**attrs)
  File "/home/herman/.cache/uv/builds-v0/.tmpg2iSa0/lib/python3.9/site-packages/setuptools/_distutils/core.py", line 184, in setup
    return run_commands(dist)
  File "/home/herman/.cache/uv/builds-v0/.tmpg2iSa0/lib/python3.9/site-packages/setuptools/_distutils/core.py", line 200, in run_commands
    dist.run_commands()
  File "/home/herman/.cache/uv/builds-v0/.tmpg2iSa0/lib/python3.9/site-packages/setuptools/_distutils/dist.py", line 964, in run_commands
    self.run_command(cmd)
  File "/home/herman/.cache/uv/builds-v0/.tmpg2iSa0/lib/python3.9/site-packages/setuptools/dist.py", line 948, in run_command
    super().run_command(command)
  File "/home/herman/.cache/uv/builds-v0/.tmpg2iSa0/lib/python3.9/site-packages/setuptools/_distutils/dist.py", line 983, in run_command
    cmd_obj.run()
  File "/home/herman/.cache/uv/builds-v0/.tmpg2iSa0/lib/python3.9/site-packages/setuptools/command/bdist_wheel.py", line 419, in run
    self.run_command("install")
  File "/home/herman/.cache/uv/builds-v0/.tmpg2iSa0/lib/python3.9/site-packages/setuptools/_distutils/cmd.py", line 316, in run_command
    self.distribution.run_command(command)
  File "/home/herman/.cache/uv/builds-v0/.tmpg2iSa0/lib/python3.9/site-packages/setuptools/dist.py", line 948, in run_command
    super().run_command(command)
  File "/home/herman/.cache/uv/builds-v0/.tmpg2iSa0/lib/python3.9/site-packages/setuptools/_distutils/dist.py", line 983, in run_command
    cmd_obj.run()
  File "<string>", line 73, in run
  File "<string>", line 42, in run_pip_command
  File "/usr/local/lib/python3.9/subprocess.py", line 373, in check_call
    raise CalledProcessError(retcode, cmd)
subprocess.CalledProcessError: Command '['/home/herman/.cache/uv/builds-v0/.tmpg2iSa0/bin/python', '-m', 'pip', 'install', '--extra-index-url', 'https://pypi.nvidia.com', 'tensorrt-cu12_libs==10.0.1', 'tensorrt-cu12_bindings==10.0.1']' returned non-zero exit status 1.
```
Here's the full log: [log.txt](https://github.com/user-attachments/files/16676448/log.txt)

However, this seems to work fine and install without `uv`

Let me know if you need any more info

---

_Comment by @zanieb on 2024-08-20 13:38_

Looks like `tensorrt-cu12` has a build requirement on `pip` that it does not declare. You'll need to use `--no-build-isolation` and install all of the build dependencies manually. This would probably fail to install via `pip` with PEP 517 enabled (`--use-pep517`). See #2252. 

---

_Label `question` added by @zanieb on 2024-08-20 13:39_

---

_Renamed from "CalledProcessError" to "Build of `tensorrt-cu12` fails with "No module named pip"" by @zanieb on 2024-08-20 13:39_

---

_Comment by @Kartstig on 2024-08-20 15:39_

wow thanks for the quick response!

---

_Comment by @zanieb on 2024-08-20 15:56_

No problem! Let me know if you have any more questions.

---

_Closed by @zanieb on 2024-08-20 18:52_

---
