```yaml
number: 4830
title: "jax[cuda12_pip] -> jax[cuda12-pip] which installs different packages"
type: issue
state: closed
author: froody
labels:
  - question
assignees: []
created_at: 2024-07-05T09:47:19Z
updated_at: 2024-07-06T19:18:11Z
url: https://github.com/astral-sh/uv/issues/4830
synced_at: 2026-01-12T15:58:52Z
```

# jax[cuda12_pip] -> jax[cuda12-pip] which installs different packages

---

_@froody_

uv rewrites `jax[cuda12_pip]==0.4.25` as `jax[cuda12-pip]==0.4.25` which results in different packages being installed by pip. IMHO it should preserve the exact spelling.

```
% cat req.in
jax[cuda12_pip]==0.4.25
% uv pip compile -v --find-links=https://storage.googleapis.com/jax-releases/jax_cuda_releases.html --no-strip-extras --no-annotate --no-header req.in

DEBUG uv 0.2.21
DEBUG Starting toolchain discovery for any Python
DEBUG Looking for exact match for request any Python
DEBUG Searching for Python interpreter in system path
DEBUG Found cpython 3.10.11 at `/home/tbirch/src/polez/conda/polez/bin/python3` (conda prefix)
DEBUG Using Python 3.10.11 interpreter at /home/tbirch/src/polez/conda/polez/bin/python3 for builds
DEBUG Using request timeout of 30s
DEBUG Found fresh response for: https://storage.googleapis.com/jax-releases/jax_cuda_releases.html
DEBUG Found 667 packages in `--find-links` entry: https://storage.googleapis.com/jax-releases/jax_cuda_releases.html
DEBUG Solving with installed Python version: 3.10.11
DEBUG Adding direct dependency: jax==0.4.25
DEBUG Adding direct dependency: jax[cuda12-pip]==0.4.25
DEBUG Found fresh response for: https://pypi.org/simple/jax/
DEBUG Searching for a compatible version of jax[cuda12-pip] (==0.4.25)
DEBUG Selecting: jax==0.4.25 (jax-0.4.25-py3-none-any.whl)
DEBUG Adding transitive dependency for jax==0.4.25: jax==0.4.25
DEBUG Adding transitive dependency for jax==0.4.25: jax[cuda12-pip]==0.4.25
DEBUG Searching for a compatible version of jax (==0.4.25)
DEBUG Selecting: jax==0.4.25 (jax-0.4.25-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ad/29/37cc2d58775917e6da532ef59cd3a66133d4de73fce1c16852e8475e5411/jax-0.4.25-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for jax==0.4.25: ml-dtypes>=0.2.0
DEBUG Adding transitive dependency for jax==0.4.25: numpy>=1.22
DEBUG Adding transitive dependency for jax==0.4.25: opt-einsum*
DEBUG Adding transitive dependency for jax==0.4.25: scipy>=1.9
DEBUG Searching for a compatible version of jax[cuda12-pip] (==0.4.25)
DEBUG Selecting: jax==0.4.25 (jax-0.4.25-py3-none-any.whl)
DEBUG Adding transitive dependency for jax==0.4.25: jaxlib==0.4.25+cuda12.cudnn89
DEBUG Adding transitive dependency for jax==0.4.25: nvidia-cublas-cu12>=12.3.4.1
DEBUG Adding transitive dependency for jax==0.4.25: nvidia-cuda-cupti-cu12>=12.3.101
DEBUG Adding transitive dependency for jax==0.4.25: nvidia-cuda-nvcc-cu12>=12.3.107
DEBUG Adding transitive dependency for jax==0.4.25: nvidia-cuda-runtime-cu12>=12.3.101
DEBUG Adding transitive dependency for jax==0.4.25: nvidia-cudnn-cu12>=8.9.7.29
DEBUG Adding transitive dependency for jax==0.4.25: nvidia-cufft-cu12>=11.0.12.1
DEBUG Adding transitive dependency for jax==0.4.25: nvidia-cusolver-cu12>=11.5.4.101
DEBUG Adding transitive dependency for jax==0.4.25: nvidia-cusparse-cu12>=12.2.0.103
DEBUG Adding transitive dependency for jax==0.4.25: nvidia-nccl-cu12>=2.19.3
DEBUG Adding transitive dependency for jax==0.4.25: nvidia-nvjitlink-cu12>=12.3.101
DEBUG Found fresh response for: https://pypi.org/simple/ml-dtypes/
DEBUG Found fresh response for: https://pypi.org/simple/opt-einsum/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cublas-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cufft-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/jaxlib/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cuda-nvcc-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cuda-runtime-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cuda-cupti-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cusparse-cu12/
DEBUG Searching for a compatible version of jaxlib (==0.4.25+cuda12.cudnn89)
DEBUG Selecting: jaxlib==0.4.25+cuda12.cudnn89 (jaxlib-0.4.25+cuda12.cudnn89-cp310-cp310-manylinux2014_x86_64.whl)
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-nvjitlink-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cudnn-cu12/
DEBUG Found fresh response for: https://pypi.org/simple/scipy/
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-cusolver-cu12/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9d/15/e5af59287e712b26ce776f00911c45c97ac9f4cd82d46500602cc94127ed/ml_dtypes-0.4.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f9/43/a5365b345c989d9f7de2f2406e59b4a792ba3541b5d47edda5031b2730a6/nvidia_cublas_cu12-12.5.3.2-py3-none-manylinux2014_x86_64.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/nvidia-nccl-cu12/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/bc/19/404708a7e54ad2798907210462fd950c3442ea51acc8790f3da48d2bee8b/opt_einsum-3.3.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e4/85/f18c88f63489cdced17b06d3b627adca8add7d7b8cce8c11213e93a902b4/nvidia_cufft_cu12-11.2.3.61-py3-none-manylinux2014_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/44/cc/36363057676e6140d0bdb07fa6df5419b68203c5cba8c412b5600fd0d105/nvidia_cuda_nvcc_cu12-12.5.82-py3-none-manylinux2014_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b3/29/03726191334fa523d9654e3dacca5cc152f24bb9fa1a5721e4dddac7a8c5/nvidia_cusparse_cu12-12.5.1.3-py3-none-manylinux2014_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/75/bc/e0d0dbb85246a086ab14839979039647bce501d8c661a159b8b019d987b7/nvidia_nvjitlink_cu12-12.5.82-py3-none-manylinux2014_x86_64.whl.metadata
DEBUG Found fresh response for: https://storage.googleapis.com/jax-releases/cuda12/jaxlib-0.4.25+cuda12.cudnn89-cp310-cp310-manylinux2014_x86_64.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/71/05/80f3fe49e9905570bc27aea9493baa1891c3780a7fc4e1f872c7902df066/nvidia_cuda_runtime_cu12-12.5.82-py3-none-manylinux2014_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/49/3e/774e7be286248bfa269fca185858b8d50b4d378aeee02d4dc5ee0b64428a/nvidia_cudnn_cu12-9.2.0.82-py3-none-manylinux2014_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e2/20/15c8fe0dfebb6facd81b3d08bf45dfa080e305deb17172b0a40eba59e927/scipy-1.14.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/33/73/57fbf55b3f378a73faecde397a0927ea205b458f06573dfb191b7d9fd1d3/nvidia_cusolver_cu12-11.6.3.83-py3-none-manylinux2014_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3a/64/81515a1b5872dc0fc817deaec2bdba160dc50188e0d53b907d10c6e6d568/nvidia_cuda_cupti_cu12-12.5.82-py3-none-manylinux2014_x86_64.whl.metadata
DEBUG Adding transitive dependency for jaxlib==0.4.25+cuda12.cudnn89: scipy>=1.9
DEBUG Adding transitive dependency for jaxlib==0.4.25+cuda12.cudnn89: numpy>=1.22
DEBUG Adding transitive dependency for jaxlib==0.4.25+cuda12.cudnn89: ml-dtypes>=0.2.0
DEBUG Searching for a compatible version of ml-dtypes (>=0.2.0)
DEBUG Selecting: ml-dtypes==0.4.0 (ml_dtypes-0.4.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for ml-dtypes==0.4.0: numpy>1.20
DEBUG Adding transitive dependency for ml-dtypes==0.4.0: numpy{python_version >= '3.10'}>=1.21.2
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/72/3a/1ff98da19ad6d54db45e5b2a1e3fb77bd19e54b75cdb20d89f8161384dbb/nvidia_nccl_cu12-2.22.3-py3-none-manylinux2014_x86_64.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/numpy/
DEBUG Searching for a compatible version of numpy (>=1.22)
DEBUG Selecting: numpy==2.0.0 (numpy-2.0.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d6/a8/6a2419c40c7b6f7cb4ef52c532c88e55490c4fa92885964757d507adddce/numpy-2.0.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Searching for a compatible version of numpy{python_version >= '3.10'} (>=1.21.2)
DEBUG Selecting: numpy==2.0.0 (numpy-2.0.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for numpy==2.0.0: numpy==2.0.0
DEBUG Adding transitive dependency for numpy==2.0.0: numpy{python_version >= '3.10'}==2.0.0
DEBUG Searching for a compatible version of numpy{python_version >= '3.10'} (==2.0.0)
DEBUG Selecting: numpy==2.0.0 (numpy-2.0.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Searching for a compatible version of opt-einsum (*)
DEBUG Selecting: opt-einsum==3.3.0 (opt_einsum-3.3.0-py3-none-any.whl)
DEBUG Adding transitive dependency for opt-einsum==3.3.0: numpy>=1.7
DEBUG Searching for a compatible version of scipy (>=1.9)
DEBUG Selecting: scipy==1.14.0 (scipy-1.14.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for scipy==1.14.0: numpy>=1.23.5, <2.3
DEBUG Searching for a compatible version of nvidia-cublas-cu12 (>=12.3.4.1)
DEBUG Selecting: nvidia-cublas-cu12==12.5.3.2 (nvidia_cublas_cu12-12.5.3.2-py3-none-manylinux2014_x86_64.whl)
DEBUG Searching for a compatible version of nvidia-cuda-cupti-cu12 (>=12.3.101)
DEBUG Selecting: nvidia-cuda-cupti-cu12==12.5.82 (nvidia_cuda_cupti_cu12-12.5.82-py3-none-manylinux2014_x86_64.whl)
DEBUG Searching for a compatible version of nvidia-cuda-nvcc-cu12 (>=12.3.107)
DEBUG Selecting: nvidia-cuda-nvcc-cu12==12.5.82 (nvidia_cuda_nvcc_cu12-12.5.82-py3-none-manylinux2014_x86_64.whl)
DEBUG Searching for a compatible version of nvidia-cuda-runtime-cu12 (>=12.3.101)
DEBUG Selecting: nvidia-cuda-runtime-cu12==12.5.82 (nvidia_cuda_runtime_cu12-12.5.82-py3-none-manylinux2014_x86_64.whl)
DEBUG Searching for a compatible version of nvidia-cudnn-cu12 (>=8.9.7.29)
DEBUG Selecting: nvidia-cudnn-cu12==9.2.0.82 (nvidia_cudnn_cu12-9.2.0.82-py3-none-manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cudnn-cu12==9.2.0.82: nvidia-cublas-cu12*
DEBUG Searching for a compatible version of nvidia-cufft-cu12 (>=11.0.12.1)
DEBUG Selecting: nvidia-cufft-cu12==11.2.3.61 (nvidia_cufft_cu12-11.2.3.61-py3-none-manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cufft-cu12==11.2.3.61: nvidia-nvjitlink-cu12*
DEBUG Searching for a compatible version of nvidia-cusolver-cu12 (>=11.5.4.101)
DEBUG Selecting: nvidia-cusolver-cu12==11.6.3.83 (nvidia_cusolver_cu12-11.6.3.83-py3-none-manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.6.3.83: nvidia-cublas-cu12*
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.6.3.83: nvidia-nvjitlink-cu12*
DEBUG Adding transitive dependency for nvidia-cusolver-cu12==11.6.3.83: nvidia-cusparse-cu12*
DEBUG Searching for a compatible version of nvidia-cusparse-cu12 (>=12.2.0.103)
DEBUG Selecting: nvidia-cusparse-cu12==12.5.1.3 (nvidia_cusparse_cu12-12.5.1.3-py3-none-manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for nvidia-cusparse-cu12==12.5.1.3: nvidia-nvjitlink-cu12*
DEBUG Searching for a compatible version of nvidia-nccl-cu12 (>=2.19.3)
DEBUG Selecting: nvidia-nccl-cu12==2.22.3 (nvidia_nccl_cu12-2.22.3-py3-none-manylinux2014_x86_64.whl)
DEBUG Searching for a compatible version of nvidia-nvjitlink-cu12 (>=12.3.101)
DEBUG Selecting: nvidia-nvjitlink-cu12==12.5.82 (nvidia_nvjitlink_cu12-12.5.82-py3-none-manylinux2014_x86_64.whl)
DEBUG Tried 16 versions: jax 1, jaxlib 1, ml-dtypes 1, numpy 1, nvidia-cublas-cu12 1, nvidia-cuda-cupti-cu12 1, nvidia-cuda-nvcc-cu12 1, nvidia-cuda-runtime-cu12 1, nvidia-cudnn-cu12 1, nvidia-cufft-cu12 1, nvidia-cusolver-cu12 1, nvidia-cusparse-cu12 1, nvidia-nccl-cu12 1, nvidia-nvjitlink-cu12 1, opt-einsum 1, scipy 1
DEBUG Split  took 0.017s
Resolved 16 packages in 21ms
jax[cuda12-pip]==0.4.25
jaxlib==0.4.25+cuda12.cudnn89
ml-dtypes==0.4.0
numpy==2.0.0
nvidia-cublas-cu12==12.5.3.2
nvidia-cuda-cupti-cu12==12.5.82
nvidia-cuda-nvcc-cu12==12.5.82
nvidia-cuda-runtime-cu12==12.5.82
nvidia-cudnn-cu12==9.2.0.82
nvidia-cufft-cu12==11.2.3.61
nvidia-cusolver-cu12==11.6.3.83
nvidia-cusparse-cu12==12.5.1.3
nvidia-nccl-cu12==2.22.3
nvidia-nvjitlink-cu12==12.5.82
opt-einsum==3.3.0
scipy==1.14.0
% python -m pip install --find-links=https://storage.googleapis.com/jax-releases/jax_cuda_releases.html 'jax[cuda12-pip]==0.4.25'
Looking in links: https://storage.googleapis.com/jax-releases/jax_cuda_releases.html
Collecting jax==0.4.25 (from jax[cuda12-pip]==0.4.25)
  Downloading jax-0.4.25-py3-none-any.whl.metadata (24 kB)
Requirement already satisfied: ml-dtypes>=0.2.0 in ./conda/polez/lib/python3.10/site-packages (from jax==0.4.25->jax[cuda12-pip]==0.4.25) (0.3.2)
Requirement already satisfied: numpy>=1.22 in ./conda/polez/lib/python3.10/site-packages (from jax==0.4.25->jax[cuda12-pip]==0.4.25) (1.26.4)
Requirement already satisfied: opt-einsum in ./conda/polez/lib/python3.10/site-packages (from jax==0.4.25->jax[cuda12-pip]==0.4.25) (3.3.0)
Requirement already satisfied: scipy>=1.9 in ./conda/polez/lib/python3.10/site-packages (from jax==0.4.25->jax[cuda12-pip]==0.4.25) (1.10.0)
WARNING: jax 0.4.25 does not provide the extra 'cuda12-pip'
Downloading jax-0.4.25-py3-none-any.whl (1.8 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.8/1.8 MB 34.8 MB/s eta 0:00:00
DEPRECATION: classifier-module e275fddd7c2b90ea6df07b7257dd3e266b52754f has a non-standard version number. pip 24.1 will enforce this behaviour change. A possible replacement is to upgrade to a newer version of classifier-module or contact the author to suggest that they release a version with a conforming version number. Discussion can be found at https://github.com/pypa/pip/issues/12063
DEPRECATION: geopolars 0.1.0a4 has a non-standard dependency specifier pyarrow>=4.0.*. pip 24.1 will enforce this behaviour change. A possible replacement is to upgrade to a newer version of geopolars or contact the author to suggest that they release a version with a conforming dependency specifiers. Discussion can be found at https://github.com/pypa/pip/issues/12063
Installing collected packages: jax
  Attempting uninstall: jax
    Found existing installation: jax 0.4.23
    Uninstalling jax-0.4.23:
      Successfully uninstalled jax-0.4.23
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
chex 0.1.85 requires jaxlib>=0.1.37, which is not installed.
optax 0.1.9 requires jaxlib>=0.1.37, which is not installed.
orbax-checkpoint 0.5.3 requires jaxlib, which is not installed.
Successfully installed jax-0.4.25

[notice] A new release of pip is available: 24.0 -> 24.1.1
[notice] To update, run: pip install --upgrade pip
% python -m pip install --find-links=https://storage.googleapis.com/jax-releases/jax_cuda_releases.html 'jax[cuda12_pip]==0.4.25'
Looking in links: https://storage.googleapis.com/jax-releases/jax_cuda_releases.html
Requirement already satisfied: jax==0.4.25 in ./conda/polez/lib/python3.10/site-packages (from jax[cuda12_pip]==0.4.25) (0.4.25)
Requirement already satisfied: ml-dtypes>=0.2.0 in ./conda/polez/lib/python3.10/site-packages (from jax==0.4.25->jax[cuda12_pip]==0.4.25) (0.3.2)
Requirement already satisfied: numpy>=1.22 in ./conda/polez/lib/python3.10/site-packages (from jax==0.4.25->jax[cuda12_pip]==0.4.25) (1.26.4)
Requirement already satisfied: opt-einsum in ./conda/polez/lib/python3.10/site-packages (from jax==0.4.25->jax[cuda12_pip]==0.4.25) (3.3.0)
Requirement already satisfied: scipy>=1.9 in ./conda/polez/lib/python3.10/site-packages (from jax==0.4.25->jax[cuda12_pip]==0.4.25) (1.10.0)
WARNING: jax 0.4.25 does not provide the extra 'cuda12-pip'
Collecting jaxlib==0.4.25+cuda12.cudnn89 (from jax[cuda12_pip]==0.4.25)
  Downloading https://storage.googleapis.com/jax-releases/cuda12/jaxlib-0.4.25%2Bcuda12.cudnn89-cp310-cp310-manylinux2014_x86_64.whl (143.5 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 143.5/143.5 MB 8.6 MB/s eta 0:00:00
Collecting nvidia-cublas-cu12>=12.3.4.1 (from jax[cuda12_pip]==0.4.25)
  Downloading nvidia_cublas_cu12-12.5.3.2-py3-none-manylinux2014_x86_64.whl.metadata (1.5 kB)
Collecting nvidia-cuda-cupti-cu12>=12.3.101 (from jax[cuda12_pip]==0.4.25)
  Downloading nvidia_cuda_cupti_cu12-12.5.82-py3-none-manylinux2014_x86_64.whl.metadata (1.6 kB)
Collecting nvidia-cuda-nvcc-cu12>=12.3.107 (from jax[cuda12_pip]==0.4.25)
  Downloading nvidia_cuda_nvcc_cu12-12.5.82-py3-none-manylinux2014_x86_64.whl.metadata (1.5 kB)
Collecting nvidia-cuda-runtime-cu12>=12.3.101 (from jax[cuda12_pip]==0.4.25)
  Downloading nvidia_cuda_runtime_cu12-12.5.82-py3-none-manylinux2014_x86_64.whl.metadata (1.5 kB)
Collecting nvidia-cudnn-cu12>=8.9.7.29 (from jax[cuda12_pip]==0.4.25)
  Downloading nvidia_cudnn_cu12-9.2.0.82-py3-none-manylinux2014_x86_64.whl.metadata (1.6 kB)
Collecting nvidia-cufft-cu12>=11.0.12.1 (from jax[cuda12_pip]==0.4.25)
  Downloading nvidia_cufft_cu12-11.2.3.61-py3-none-manylinux2014_x86_64.whl.metadata (1.5 kB)
Collecting nvidia-cusolver-cu12>=11.5.4.101 (from jax[cuda12_pip]==0.4.25)
  Downloading nvidia_cusolver_cu12-11.6.3.83-py3-none-manylinux2014_x86_64.whl.metadata (1.6 kB)
Collecting nvidia-cusparse-cu12>=12.2.0.103 (from jax[cuda12_pip]==0.4.25)
  Downloading nvidia_cusparse_cu12-12.5.1.3-py3-none-manylinux2014_x86_64.whl.metadata (1.6 kB)
Collecting nvidia-nccl-cu12>=2.19.3 (from jax[cuda12_pip]==0.4.25)
  Downloading nvidia_nccl_cu12-2.22.3-py3-none-manylinux2014_x86_64.whl.metadata (1.8 kB)
Collecting nvidia-nvjitlink-cu12>=12.3.101 (from jax[cuda12_pip]==0.4.25)
  Downloading nvidia_nvjitlink_cu12-12.5.82-py3-none-manylinux2014_x86_64.whl.metadata (1.5 kB)
Downloading nvidia_cublas_cu12-12.5.3.2-py3-none-manylinux2014_x86_64.whl (363.3 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 363.3/363.3 MB 10.9 MB/s eta 0:00:00
Downloading nvidia_cuda_cupti_cu12-12.5.82-py3-none-manylinux2014_x86_64.whl (13.8 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 13.8/13.8 MB 68.2 MB/s eta 0:00:00
Downloading nvidia_cuda_nvcc_cu12-12.5.82-py3-none-manylinux2014_x86_64.whl (22.5 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 22.5/22.5 MB 22.7 MB/s eta 0:00:00
Downloading nvidia_cuda_runtime_cu12-12.5.82-py3-none-manylinux2014_x86_64.whl (895 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 895.7/895.7 kB 62.9 MB/s eta 0:00:00
Downloading nvidia_cudnn_cu12-9.2.0.82-py3-none-manylinux2014_x86_64.whl (574.6 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 574.6/574.6 MB 7.9 MB/s eta 0:00:00
Downloading nvidia_cufft_cu12-11.2.3.61-py3-none-manylinux2014_x86_64.whl (192.5 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 192.5/192.5 MB 14.2 MB/s eta 0:00:00
Downloading nvidia_cusolver_cu12-11.6.3.83-py3-none-manylinux2014_x86_64.whl (130.3 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 130.3/130.3 MB 22.8 MB/s eta 0:00:00
Downloading nvidia_cusparse_cu12-12.5.1.3-py3-none-manylinux2014_x86_64.whl (217.6 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 217.6/217.6 MB 13.5 MB/s eta 0:00:00
Downloading nvidia_nccl_cu12-2.22.3-py3-none-manylinux2014_x86_64.whl (190.9 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 190.9/190.9 MB 22.3 MB/s eta 0:00:00
Downloading nvidia_nvjitlink_cu12-12.5.82-py3-none-manylinux2014_x86_64.whl (21.3 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 21.3/21.3 MB 63.0 MB/s eta 0:00:00
DEPRECATION: classifier-module e275fddd7c2b90ea6df07b7257dd3e266b52754f has a non-standard version number. pip 24.1 will enforce this behaviour change. A possible replacement is to upgrade to a newer version of classifier-module or contact the author to suggest that they release a version with a conforming version number. Discussion can be found at https://github.com/pypa/pip/issues/12063
DEPRECATION: geopolars 0.1.0a4 has a non-standard dependency specifier pyarrow>=4.0.*. pip 24.1 will enforce this behaviour change. A possible replacement is to upgrade to a newer version of geopolars or contact the author to suggest that they release a version with a conforming dependency specifiers. Discussion can be found at https://github.com/pypa/pip/issues/12063
Installing collected packages: nvidia-nvjitlink-cu12, nvidia-nccl-cu12, nvidia-cuda-runtime-cu12, nvidia-cuda-nvcc-cu12, nvidia-cuda-cupti-cu12, nvidia-cublas-cu12, nvidia-cusparse-cu12, nvidia-cufft-cu12, nvidia-cudnn-cu12, jaxlib, nvidia-cusolver-cu12
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
torch 2.2.0 requires nvidia-cuda-nvrtc-cu12==12.1.105; platform_system == "Linux" and platform_machine == "x86_64", which is not installed.
torch 2.2.0 requires nvidia-curand-cu12==10.3.2.106; platform_system == "Linux" and platform_machine == "x86_64", which is not installed.
torch 2.2.0 requires nvidia-nvtx-cu12==12.1.105; platform_system == "Linux" and platform_machine == "x86_64", which is not installed.
torch 2.2.0 requires nvidia-cublas-cu12==12.1.3.1; platform_system == "Linux" and platform_machine == "x86_64", but you have nvidia-cublas-cu12 12.5.3.2 which is incompatible.
torch 2.2.0 requires nvidia-cuda-cupti-cu12==12.1.105; platform_system == "Linux" and platform_machine == "x86_64", but you have nvidia-cuda-cupti-cu12 12.5.82 which is incompatible.
torch 2.2.0 requires nvidia-cuda-runtime-cu12==12.1.105; platform_system == "Linux" and platform_machine == "x86_64", but you have nvidia-cuda-runtime-cu12 12.5.82 which is incompatible.
torch 2.2.0 requires nvidia-cudnn-cu12==8.9.2.26; platform_system == "Linux" and platform_machine == "x86_64", but you have nvidia-cudnn-cu12 9.2.0.82 which is incompatible.
torch 2.2.0 requires nvidia-cufft-cu12==11.0.2.54; platform_system == "Linux" and platform_machine == "x86_64", but you have nvidia-cufft-cu12 11.2.3.61 which is incompatible.
torch 2.2.0 requires nvidia-cusolver-cu12==11.4.5.107; platform_system == "Linux" and platform_machine == "x86_64", but you have nvidia-cusolver-cu12 11.6.3.83 which is incompatible.
torch 2.2.0 requires nvidia-cusparse-cu12==12.1.0.106; platform_system == "Linux" and platform_machine == "x86_64", but you have nvidia-cusparse-cu12 12.5.1.3 which is incompatible.
torch 2.2.0 requires nvidia-nccl-cu12==2.19.3; platform_system == "Linux" and platform_machine == "x86_64", but you have nvidia-nccl-cu12 2.22.3 which is incompatible.
Successfully installed jaxlib-0.4.25+cuda12.cudnn89 nvidia-cublas-cu12-12.5.3.2 nvidia-cuda-cupti-cu12-12.5.82 nvidia-cuda-nvcc-cu12-12.5.82 nvidia-cuda-runtime-cu12-12.5.82 nvidia-cudnn-cu12-9.2.0.82 nvidia-cufft-cu12-11.2.3.61 nvidia-cusolver-cu12-11.6.3.83 nvidia-cusparse-cu12-12.5.1.3 nvidia-nccl-cu12-2.22.3 nvidia-nvjitlink-cu12-12.5.82

[notice] A new release of pip is available: 24.0 -> 24.1.1
[notice] To update, run: pip install --upgrade pip
```

---

_Comment by @zanieb on 2024-07-05 17:05_

Hi! Please see the [PEP 685](https://peps.python.org/pep-0685/#specification) specification which says:

> When comparing extra names, tools MUST normalize the names being compared using the semantics outlined in [PEP 503](https://peps.python.org/pep-0503/#normalized-names) for names:
> 
> ```
> re.sub(r"[-_.]+", "-", name).lower()
> ```

Using and displaying `-` is spec-compliant. Maybe something else is going wrong?

---

_Comment by @zanieb on 2024-07-05 17:08_

Could you split up your examples a bit with some explanation of what each invocation is demonstrating? It's pretty hard to understand as-is.

---

_Comment by @notatallshaw on 2024-07-05 20:25_

Yeah, this is an issue with pip, not uv, and is fixed in pip 24.1, as the notice says, update your pip:

```
[notice] A new release of pip is available: 24.0 -> 24.1.1
[notice] To update, run: pip install --upgrade pip
```

Or use uv to install, rather than pip.

---

_Label `question` added by @charliermarsh on 2024-07-05 20:29_

---

_Comment by @charliermarsh on 2024-07-05 20:29_

Thanks @notatallshaw, appreciate the confirmation.

---

_Closed by @charliermarsh on 2024-07-06 19:18_

---
