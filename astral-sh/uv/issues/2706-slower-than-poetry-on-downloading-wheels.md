---
number: 2706
title: slower than poetry on downloading wheels
type: issue
state: closed
author: messense
labels:
  - performance
assignees: []
created_at: 2024-03-28T05:58:46Z
updated_at: 2024-05-21T06:31:26Z
url: https://github.com/astral-sh/uv/issues/2706
synced_at: 2026-01-10T01:23:21Z
---

# slower than poetry on downloading wheels

---

_Issue opened by @messense on 2024-03-28 05:58_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Comparing `poetry install` with `uv install` a `poetry export`ed `requirments.txt`:

* `poetry` takes 4m5s, peak network speed is 528 Mibps
* `uv` takes 4m17s, peak network speed is 229 Mibps (Resolve is fast, only took 1.39s)

```
Resolved 173 packages in 1.39s
Downloaded 173 packages in 4m 15s
Installed 173 packages in 141ms
```

It seems that `uv` does not download wheels in parallel yet, and it's a bottleneck for some use cases.

---

uv platform: Ubuntu 22.04
uv version: 0.1.24


---

_Label `performance` added by @AlexWaygood on 2024-03-28 06:40_

---

_Comment by @konstin on 2024-03-28 08:54_

Can you provide the requirements.txt or a similar one that reproduces that problem?

For context, we download and streaming unzip the wheels in parallel, maybe it's the unzipping that makes it look slower?

---

_Comment by @messense on 2024-03-28 08:57_

Here you go:

<details>

```
--find-links https://storage.googleapis.com/jax-releases/jax_cuda_releases.html

aiobotocore==2.5.4 ; python_version >= "3.10" and python_version < "3.11"
aiohttp-cors==0.7.0 ; python_version >= "3.10" and python_version < "3.11"
aiohttp==3.8.5 ; python_version >= "3.10" and python_version < "3.11"
aioitertools==0.11.0 ; python_version >= "3.10" and python_version < "3.11"
aiosignal==1.3.1 ; python_version >= "3.10" and python_version < "3.11"
appnope==0.1.3 ; python_version >= "3.10" and python_version < "3.11" and sys_platform == "darwin"
asttokens==2.2.1 ; python_version >= "3.10" and python_version < "3.11"
async-timeout==4.0.3 ; python_version >= "3.10" and python_version < "3.11"
attrs==23.1.0 ; python_version >= "3.10" and python_version < "3.11"
backcall==0.2.0 ; python_version >= "3.10" and python_version < "3.11"
blinker==1.7.0 ; python_version >= "3.10" and python_version < "3.11"
boto3==1.28.17 ; python_version >= "3.10" and python_version < "3.11"
botocore==1.31.17 ; python_version >= "3.10" and python_version < "3.11"
bottleneck==1.3.7 ; python_version >= "3.10" and python_version < "3.11"
cachetools==5.3.1 ; python_version >= "3.10" and python_version < "3.11"
certifi==2023.7.22 ; python_version >= "3.10" and python_version < "3.11"
cffi==1.15.1 ; python_version >= "3.10" and python_version < "3.11" and platform_python_implementation == "PyPy"
cftime==1.6.2 ; python_version >= "3.10" and python_version < "3.11"
charset-normalizer==3.2.0 ; python_version >= "3.10" and python_version < "3.11"
clarabel==0.5.1 ; python_version >= "3.10" and python_version < "3.11"
click==8.1.7 ; python_version >= "3.10" and python_version < "3.11"
cloudpathlib[s3]==0.15.1 ; python_version >= "3.10" and python_version < "3.11"
cloudpickle==2.2.1 ; python_version >= "3.10" and python_version < "3.11"
colorama==0.4.6 ; python_version >= "3.10" and python_version < "3.11" and (platform_system == "Windows" or sys_platform == "win32")
colorful==0.5.5 ; python_version >= "3.10" and python_version < "3.11"
comm==0.1.4 ; python_version >= "3.10" and python_version < "3.11"
contourpy==1.1.0 ; python_version >= "3.10" and python_version < "3.11"
cvxpy==1.3.2 ; python_version >= "3.10" and python_version < "3.11"
cycler==0.11.0 ; python_version >= "3.10" and python_version < "3.11"
debugpy==1.8.0 ; python_version >= "3.10" and python_version < "3.11"
decorator==5.1.1 ; python_version >= "3.10" and python_version < "3.11"
dill==0.3.8 ; python_version >= "3.10" and python_version < "3.11"
distlib==0.3.7 ; python_version >= "3.10" and python_version < "3.11"
dm-tree==0.1.8 ; python_version >= "3.10" and python_version < "3.11"
ecos==2.0.12 ; python_version >= "3.10" and python_version < "3.11"
einops==0.7.0 ; python_version >= "3.10" and python_version < "3.11"
etcd3 @ https://github.com/kragniz/python-etcd3/archive/e58a899579ba416449c4e225b61f039457c8072a.zip ; python_version >= "3.10" and python_version < "3.11"
executing==1.2.0 ; python_version >= "3.10" and python_version < "3.11"
filelock==3.13.1 ; python_version >= "3.10" and python_version < "3.11"
flask==3.0.0 ; python_version >= "3.10" and python_version < "3.11"
fonttools==4.42.1 ; python_version >= "3.10" and python_version < "3.11"
frozendict==2.3.10 ; python_version >= "3.10" and python_version < "3.11"
frozenlist==1.4.0 ; python_version >= "3.10" and python_version < "3.11"
fsspec==2023.6.0 ; python_version >= "3.10" and python_version < "3.11"
google-api-core==2.11.1 ; python_version >= "3.10" and python_version < "3.11"
google-auth==2.22.0 ; python_version >= "3.10" and python_version < "3.11"
googleapis-common-protos==1.60.0 ; python_version >= "3.10" and python_version < "3.11"
graphviz==0.20.1 ; python_version >= "3.10" and python_version < "3.11"
grpcio==1.59.2 ; python_version >= "3.10" and python_version < "3.11"
idna==3.4 ; python_version >= "3.10" and python_version < "3.11"
ipython==8.14.0 ; python_version >= "3.10" and python_version < "3.11"
ipywidgets==8.1.0 ; python_version >= "3.10" and python_version < "3.11"
itsdangerous==2.1.2 ; python_version >= "3.10" and python_version < "3.11"
jax[cuda12-pip]==0.4.16 ; python_version >= "3.10" and python_version < "3.11"
jaxlib==0.4.16+cuda12.cudnn89 ; python_version >= "3.10" and python_version < "3.11"
jedi==0.19.0 ; python_version >= "3.10" and python_version < "3.11"
jinja2==3.1.2 ; python_version >= "3.10" and python_version < "3.11"
jmespath==1.0.1 ; python_version >= "3.10" and python_version < "3.11"
joblib==1.3.2 ; python_version >= "3.10" and python_version < "3.11"
jsonschema-specifications==2023.7.1 ; python_version >= "3.10" and python_version < "3.11"
jsonschema==4.19.0 ; python_version >= "3.10" and python_version < "3.11"
jupyterlab-widgets==3.0.8 ; python_version >= "3.10" and python_version < "3.11"
kaleido==0.2.1 ; python_version >= "3.10" and python_version < "3.11"
kiwisolver==1.4.4 ; python_version >= "3.10" and python_version < "3.11"
lightgbm==4.2.0 ; python_version >= "3.10" and python_version < "3.11"
lightning-utilities==0.11.0 ; python_version >= "3.10" and python_version < "3.11"
llvmlite==0.40.1 ; python_version >= "3.10" and python_version < "3.11"
markupsafe==2.1.3 ; python_version >= "3.10" and python_version < "3.11"
mashumaro[msgpack]==3.10 ; python_version >= "3.10" and python_version < "3.11"
matplotlib-inline==0.1.6 ; python_version >= "3.10" and python_version < "3.11"
matplotlib==3.7.2 ; python_version >= "3.10" and python_version < "3.11"
ml-dtypes==0.3.1 ; python_version >= "3.10" and python_version < "3.11"
mpmath==1.3.0 ; python_version >= "3.10" and python_version < "3.11"
msgpack==1.0.5 ; python_version >= "3.10" and python_version < "3.11"
multidict==6.0.4 ; python_version >= "3.10" and python_version < "3.11"
multiprocess==0.70.16 ; python_version >= "3.10" and python_version < "3.11"
netcdf4==1.6.4 ; python_version >= "3.10" and python_version < "3.11"
networkx==3.1 ; python_version >= "3.10" and python_version < "3.11"
numba==0.57.1 ; python_version >= "3.10" and python_version < "3.11"
numexpr==2.8.7 ; python_version >= "3.10" and python_version < "3.11"
numpy==1.24.4 ; python_version >= "3.10" and python_version < "3.11"
nvidia-cublas-cu12==12.1.3.1 ; python_version >= "3.10" and python_version < "3.11"
nvidia-cuda-cupti-cu12==12.1.105 ; python_version >= "3.10" and python_version < "3.11"
nvidia-cuda-nvcc-cu12==12.4.99 ; python_version >= "3.10" and python_version < "3.11"
nvidia-cuda-nvrtc-cu12==12.1.105 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version >= "3.10" and python_version < "3.11"
nvidia-cuda-runtime-cu12==12.1.105 ; python_version >= "3.10" and python_version < "3.11"
nvidia-cudnn-cu12==8.9.2.26 ; python_version >= "3.10" and python_version < "3.11"
nvidia-cufft-cu12==11.0.2.54 ; python_version >= "3.10" and python_version < "3.11"
nvidia-curand-cu12==10.3.2.106 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version >= "3.10" and python_version < "3.11"
nvidia-cusolver-cu12==11.4.5.107 ; python_version >= "3.10" and python_version < "3.11"
nvidia-cusparse-cu12==12.1.0.106 ; python_version >= "3.10" and python_version < "3.11"
nvidia-nccl-cu12==2.19.3 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version >= "3.10" and python_version < "3.11"
nvidia-nvjitlink-cu12==12.3.101 ; python_version >= "3.10" and python_version < "3.11"
nvidia-nvtx-cu12==12.1.105 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version >= "3.10" and python_version < "3.11"
opencensus-context==0.1.3 ; python_version >= "3.10" and python_version < "3.11"
opencensus==0.11.2 ; python_version >= "3.10" and python_version < "3.11"
opt-einsum==3.3.0 ; python_version >= "3.10" and python_version < "3.11"
osqp==0.6.3 ; python_version >= "3.10" and python_version < "3.11"
packaging==23.1 ; python_version >= "3.10" and python_version < "3.11"
pandas==2.1.2 ; python_version >= "3.10" and python_version < "3.11"
pandas[aws,computation,parquet,performance]==2.1.2 ; python_version >= "3.10" and python_version < "3.11"
parso==0.8.3 ; python_version >= "3.10" and python_version < "3.11"
pathos==0.3.2 ; python_version >= "3.10" and python_version < "3.11"
patsy==0.5.6 ; python_version >= "3.10" and python_version < "3.11"
pendulum==2.1.2 ; python_version >= "3.10" and python_version < "3.11"
pexpect==4.8.0 ; python_version >= "3.10" and python_version < "3.11" and sys_platform != "win32"
pickleshare==0.7.5 ; python_version >= "3.10" and python_version < "3.11"
pillow==10.0.0 ; python_version >= "3.10" and python_version < "3.11"
platformdirs==3.10.0 ; python_version >= "3.10" and python_version < "3.11"
plotly==5.16.1 ; python_version >= "3.10" and python_version < "3.11"
polars==0.20.13 ; python_version >= "3.10" and python_version < "3.11"
pox==0.3.4 ; python_version >= "3.10" and python_version < "3.11"
ppft==1.7.6.8 ; python_version >= "3.10" and python_version < "3.11"
prometheus-client==0.19.0 ; python_version >= "3.10" and python_version < "3.11"
prompt-toolkit==3.0.39 ; python_version >= "3.10" and python_version < "3.11"
protobuf==4.25.0 ; python_version >= "3.10" and python_version < "3.11"
psutil==5.9.6 ; python_version >= "3.10" and python_version < "3.11"
ptyprocess==0.7.0 ; python_version >= "3.10" and python_version < "3.11" and sys_platform != "win32"
pure-eval==0.2.2 ; python_version >= "3.10" and python_version < "3.11"
py-spy==0.3.14 ; python_version >= "3.10" and python_version < "3.11"
pyarrow==15.0.0 ; python_version >= "3.10" and python_version < "3.11"
pyasn1-modules==0.3.0 ; python_version >= "3.10" and python_version < "3.11"
pyasn1==0.5.0 ; python_version >= "3.10" and python_version < "3.11"
pycparser==2.21 ; python_version >= "3.10" and python_version < "3.11" and platform_python_implementation == "PyPy"
pydantic==1.10.13 ; python_version >= "3.10" and python_version < "3.11"
pygments==2.16.1 ; python_version >= "3.10" and python_version < "3.11"
pyinstrument==4.5.1 ; python_version >= "3.10" and python_version < "3.11"
pyparsing==3.0.9 ; python_version >= "3.10" and python_version < "3.11"
python-dateutil==2.8.2 ; python_version >= "3.10" and python_version < "3.11"
pytz==2023.3 ; python_version >= "3.10" and python_version < "3.11"
pytzdata==2020.1 ; python_version >= "3.10" and python_version < "3.11"
pyyaml==6.0.1 ; python_version >= "3.10" and python_version < "3.11"
qdldl==0.1.7.post0 ; python_version >= "3.10" and python_version < "3.11"
ray[default,tune]==2.10.0 ; python_version >= "3.10" and python_version < "3.11"
referencing==0.30.2 ; python_version >= "3.10" and python_version < "3.11"
requests==2.31.0 ; python_version >= "3.10" and python_version < "3.11"
rpds-py==0.9.2 ; python_version >= "3.10" and python_version < "3.11"
rsa==4.9 ; python_version >= "3.10" and python_version < "3.11"
s3fs==2023.6.0 ; python_version >= "3.10" and python_version < "3.11"
s3transfer==0.6.2 ; python_version >= "3.10" and python_version < "3.11"
scikit-learn==1.3.0 ; python_version >= "3.10" and python_version < "3.11"
scipy==1.11.2 ; python_version >= "3.10" and python_version < "3.11"
scs==3.2.3 ; python_version >= "3.10" and python_version < "3.11"
seaborn==0.12.2 ; python_version >= "3.10" and python_version < "3.11"
setproctitle==1.3.2 ; python_version >= "3.10" and python_version < "3.11"
setuptools==68.1.2 ; python_version >= "3.10" and python_version < "3.11"
shap==0.44.0 ; python_version >= "3.10" and python_version < "3.11"
sitecustomize-entrypoints==1.1.0 ; python_version >= "3.10" and python_version < "3.11"
six==1.16.0 ; python_version >= "3.10" and python_version < "3.11"
slack-bolt==1.18.1 ; python_version >= "3.10" and python_version < "3.11"
slack-sdk==3.26.0 ; python_version >= "3.10" and python_version < "3.11"
slicer==0.0.7 ; python_version >= "3.10" and python_version < "3.11"
smart-open==6.3.0 ; python_version >= "3.10" and python_version < "3.11"
stack-data==0.6.2 ; python_version >= "3.10" and python_version < "3.11"
statsmodels==0.14.1 ; python_version >= "3.10" and python_version < "3.11"
sympy==1.12 ; python_version >= "3.10" and python_version < "3.11"
tabulate==0.9.0 ; python_version >= "3.10" and python_version < "3.11"
tenacity==8.2.3 ; python_version >= "3.10" and python_version < "3.11"
tensorboardx==2.6.2.2 ; python_version >= "3.10" and python_version < "3.11"
threadpoolctl==3.2.0 ; python_version >= "3.10" and python_version < "3.11"
torch==2.2.1 ; python_version >= "3.10" and python_version < "3.11"
torchmetrics==1.3.2 ; python_version >= "3.10" and python_version < "3.11"
tqdm==4.66.1 ; python_version >= "3.10" and python_version < "3.11"
traitlets==5.9.0 ; python_version >= "3.10" and python_version < "3.11"
triton==2.2.0 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version < "3.11" and python_version >= "3.10"
typing-extensions==4.10.0 ; python_version >= "3.10" and python_version < "3.11"
tzdata==2023.3 ; python_version >= "3.10" and python_version < "3.11"
urllib3==1.26.16 ; python_version >= "3.10" and python_version < "3.11"
virtualenv==20.21.0 ; python_version >= "3.10" and python_version < "3.11"
wcwidth==0.2.6 ; python_version >= "3.10" and python_version < "3.11"
werkzeug==3.0.1 ; python_version >= "3.10" and python_version < "3.11"
widgetsnbextension==4.0.8 ; python_version >= "3.10" and python_version < "3.11"
wrapt==1.15.0 ; python_version >= "3.10" and python_version < "3.11"
xarray==2023.8.0 ; python_version >= "3.10" and python_version < "3.11"
xgboost==2.0.2 ; python_version >= "3.10" and python_version < "3.11"
xlrd==2.0.1 ; python_version >= "3.10" and python_version < "3.11"
yarl==1.9.2 ; python_version >= "3.10" and python_version < "3.11"
zstandard==0.22.0 ; python_version >= "3.10" and python_version < "3.11"
```

</details>

---

_Comment by @messense on 2024-03-28 09:01_

> For context, we download and streaming unzip the wheels in parallel, maybe it's the unzipping that makes it look slower?

From the network usage, it seems that `uv` is only downloading one wheel at a time. 

All downloads go through a HTTP proxy, the `uv install` network speed is very close to download a big wheel (for example `pytorch`) from pypi using `wget`.

---

_Comment by @charliermarsh on 2024-03-28 18:25_

The 4m15s is combined for downloading and unzipping. Unzipping many of those wheels is extremely slow, so it's not... _totally_ surprising. Though if you're only seeing a single download at a time, that's confusing.

---

_Comment by @charliermarsh on 2024-03-28 18:28_

> From the network usage, it seems that uv is only downloading one wheel at a time.

How did you come to this conclusion? What are you observing?

---

_Comment by @charliermarsh on 2024-03-28 18:42_

Separately, have you confirmed that your proxy isn't single-threaded?

---

_Assigned to @konstin by @konstin on 2024-03-28 18:56_

---

_Comment by @messense on 2024-03-28 21:54_

> Separately, have you confirmed that your proxy isn't single-threaded?

Yes, it is not. Poetry go through the same proxy to download and unzip wheels but takes less time with higher network utilization.

---

_Comment by @charliermarsh on 2024-03-28 22:00_

How are you measuring the time it takes for Poetry to download _and_ unzip the wheels? How are you separating that from the time it takes for Poetry to install the wheels? 

---

_Comment by @charliermarsh on 2024-03-28 22:08_

E.g., Poetry doesn't unzip as it downloads. It downloads zipped wheels, then passes zipped wheels to https://github.com/pypa/installer. So you can't directly measure the "download time" between the two tools, uv is doing significantly more work. (I'm mostly trying to understand how you determined that uv isn't downloading in parallel.)

---

_Comment by @messense on 2024-03-28 22:20_

I'm on mobile so can't provide more details, will post more info next week.

---

_Comment by @charliermarsh on 2024-03-28 22:21_

Thanks very much, I'd like to improve this if we can.

---

_Comment by @messense on 2024-04-02 08:00_

I'm unable to post images here on work computer, but the symptom is that `poetry` can utilize twice more network resources than `uv`, and `uv` CPU usage is around 1%.

---

_Comment by @charliermarsh on 2024-04-02 12:29_

Strange... We definitely download in parallel so not sure why the network usage looks different. My guess is it's related to our download+unzip behavior but I don't know.

@konstin will look into it.


---

_Unassigned @konstin by @konstin on 2024-04-29 14:58_

---

_Assigned to @ibraheemdev by @konstin on 2024-04-29 14:58_

---

_Comment by @ibraheemdev on 2024-04-30 18:46_

On my machine the downloads complete in 3m 18s. We are definitely downloading in parallel, I can see the progress running from the https://github.com/astral-sh/uv/pull/3252 branch. @messense can you provide the `pyproject.toml` you used to install with poetry?

---

_Comment by @messense on 2024-05-13 09:24_

I don't have the `pyproject.toml` at hand, but you can probably using `poetry add` command to add everything in the `requirements.txt` to a `pyproject.toml`.

BTW, 3m18s still feels slow, does it fully utilize your network?

---

_Comment by @ibraheemdev on 2024-05-16 17:02_

@messense Here's the progress output:

https://github.com/astral-sh/uv/assets/34988408/26b288de-d17a-4544-bfa5-c993529e8780

You can see we finish the smaller downloads relatively quickly. We're really limited by the slowest download here, I'm not sure there's much we can do better.

---

_Comment by @messense on 2024-05-21 06:31_

Closing now that it only takes ~1m11s in my environment using uv 0.1.45.

---

_Closed by @messense on 2024-05-21 06:31_

---

_Referenced in [astral-sh/uv#3252](../../astral-sh/uv/pulls/3252.md) on 2024-05-23 15:28_

---
