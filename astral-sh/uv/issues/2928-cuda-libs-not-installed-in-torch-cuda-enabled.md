---
number: 2928
title: CUDA libs not installed in Torch CUDA-enabled Docker
type: issue
state: closed
author: avishniakov
labels: []
assignees: []
created_at: 2024-04-09T08:05:19Z
updated_at: 2024-05-14T07:10:05Z
url: https://github.com/astral-sh/uv/issues/2928
synced_at: 2026-01-10T01:23:23Z
---

# CUDA libs not installed in Torch CUDA-enabled Docker

---

_Issue opened by @avishniakov on 2024-04-09 08:05_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hello dear UV maintainers,

I faced a mismatch in the installed versions of packages in the Docker environment which is affecting it to work properly with CUDA payloads (I used Vertex AI A100 accelerated jobs specifically). At the same time, PIP was able to resolve dependencies as expected and the produced image was ready to use in the CUDA environment.

## TL;DR;
UV reports in output of `uv pip install ...` that following is installed:
```diff
- networkx==3.1 (from file:///croot/networkx_1690561992265/work)
+ networkx==3.2.1
+ nvidia-cublas-cu12==12.1.3.1
+ nvidia-cuda-cupti-cu12==12.1.105
+ nvidia-cuda-nvrtc-cu12==12.1.105
+ nvidia-cuda-runtime-cu12==12.1.105
+ nvidia-cudnn-cu12==8.9.2.26
+ nvidia-cufft-cu12==11.0.2.54
+ nvidia-curand-cu12==10.3.2.106
+ nvidia-cusolver-cu12==11.4.5.107
+ nvidia-cusparse-cu12==12.1.0.106
+ nvidia-nccl-cu12==2.19.3
+ nvidia-nvjitlink-cu12==12.4.127
+ nvidia-nvtx-cu12==12.1.105
```

PIP instal has these in `pip freeze`, while UV doesn't:
```
networkx==3.2.1
nvidia-cublas-cu12==12.1.3.1
nvidia-cuda-cupti-cu12==12.1.105
nvidia-cuda-nvrtc-cu12==12.1.105
nvidia-cuda-runtime-cu12==12.1.105
nvidia-cudnn-cu12==8.9.2.26
nvidia-cufft-cu12==11.0.2.54
nvidia-curand-cu12==10.3.2.106
nvidia-cusolver-cu12==11.4.5.107
nvidia-cusparse-cu12==12.1.0.106
nvidia-nccl-cu12==2.19.3
nvidia-nvjitlink-cu12==12.4.127
nvidia-nvtx-cu12==12.1.105
```

## Steps to reproduce using UV:
```bash
> docker run -it pytorch/pytorch:2.2.2-cuda12.1-cudnn8-runtime bash
> pip install uv
> uv pip install "torch>=2.2.0" datasets transformers peft "bitsandbytes==0.41.0" scipy --verbose --system
> pip freeze
pip freeze
accelerate==0.29.1
aiohttp==3.9.3
aiosignal==1.3.1
archspec @ file:///croot/archspec_1709217642129/work
asttokens @ file:///opt/conda/conda-bld/asttokens_1646925590279/work
astunparse==1.6.3
async-timeout==4.0.3
attrs @ file:///croot/attrs_1695717823297/work
beautifulsoup4 @ file:///croot/beautifulsoup4-split_1681493039619/work
bitsandbytes==0.41.0
boltons @ file:///croot/boltons_1677628692245/work
Brotli @ file:///tmp/abs_ecyw11_7ze/croots/recipe/brotli-split_1659616059936/work
certifi @ file:///croot/certifi_1707229174982/work/certifi
cffi @ file:///croot/cffi_1700254295673/work
chardet @ file:///home/builder/ci_310/chardet_1640804867535/work
charset-normalizer @ file:///tmp/build/80754af9/charset-normalizer_1630003229654/work
click @ file:///croot/click_1698129812380/work
conda @ file:///croot/conda_1696257509808/work
conda-build @ file:///croot/conda-build_1710789183177/work
conda-content-trust @ file:///croot/conda-content-trust_1693490622020/work
conda-libmamba-solver @ file:///croot/conda-libmamba-solver_1691418897561/work/src
conda-package-handling @ file:///croot/conda-package-handling_1690999929514/work
conda_index @ file:///croot/conda-index_1706633791028/work
conda_package_streaming @ file:///croot/conda-package-streaming_1690987966409/work
cryptography @ file:///croot/cryptography_1710350347627/work
datasets==2.18.0
decorator @ file:///opt/conda/conda-bld/decorator_1643638310831/work
dill==0.3.8
distro @ file:///croot/distro_1701455004953/work
dnspython==2.6.1
exceptiongroup @ file:///croot/exceptiongroup_1706031385326/work
executing @ file:///opt/conda/conda-bld/executing_1646925071911/work
expecttest==0.2.1
filelock @ file:///croot/filelock_1700591183607/work
frozenlist==1.4.1
fsspec==2024.2.0
gmpy2 @ file:///tmp/build/80754af9/gmpy2_1645455533097/work
huggingface-hub==0.22.2
hypothesis==6.99.13
idna @ file:///croot/idna_1666125576474/work
ipython @ file:///croot/ipython_1704833016303/work
jedi @ file:///tmp/build/80754af9/jedi_1644315229345/work
Jinja2 @ file:///croot/jinja2_1706733616596/work
jsonpatch @ file:///tmp/build/80754af9/jsonpatch_1615747632069/work
jsonpointer==2.1
jsonschema @ file:///croot/jsonschema_1699041609003/work
jsonschema-specifications @ file:///croot/jsonschema-specifications_1699032386549/work
libarchive-c @ file:///tmp/build/80754af9/python-libarchive-c_1617780486945/work
libmambapy @ file:///croot/mamba-split_1698782620632/work/libmambapy
MarkupSafe @ file:///croot/markupsafe_1704205993651/work
matplotlib-inline @ file:///opt/conda/conda-bld/matplotlib-inline_1662014470464/work
menuinst @ file:///croot/menuinst_1706732933928/work
mkl-fft @ file:///croot/mkl_fft_1695058164594/work
mkl-random @ file:///croot/mkl_random_1695059800811/work
mkl-service==2.4.0
more-itertools @ file:///croot/more-itertools_1700662129964/work
mpmath @ file:///croot/mpmath_1690848262763/work
multidict==6.0.5
multiprocess==0.70.16
networkx==3.2.1
numpy @ file:///croot/numpy_and_numpy_base_1708638617955/work/dist/numpy-1.26.4-cp310-cp310-linux_x86_64.whl#sha256=d8cd837ed43e87f77e6efaa08e8de927ca030a1c9c5d04624432d6fb9a74a5ee
nvidia-cublas-cu12==12.1.3.1
nvidia-cuda-cupti-cu12==12.1.105
nvidia-cuda-nvrtc-cu12==12.1.105
nvidia-cuda-runtime-cu12==12.1.105
nvidia-cudnn-cu12==8.9.2.26
nvidia-cufft-cu12==11.0.2.54
nvidia-curand-cu12==10.3.2.106
nvidia-cusolver-cu12==11.4.5.107
nvidia-cusparse-cu12==12.1.0.106
nvidia-nccl-cu12==2.19.3
nvidia-nvjitlink-cu12==12.4.127
nvidia-nvtx-cu12==12.1.105
optree==0.11.0
packaging @ file:///croot/packaging_1710807400464/work
pandas==2.2.1
parso @ file:///opt/conda/conda-bld/parso_1641458642106/work
peft==0.10.0
pexpect @ file:///tmp/build/80754af9/pexpect_1605563209008/work
pillow @ file:///croot/pillow_1707233021655/work
pkginfo @ file:///croot/pkginfo_1679431160147/work
platformdirs @ file:///croot/platformdirs_1692205439124/work
pluggy @ file:///tmp/build/80754af9/pluggy_1648024709248/work
prompt-toolkit @ file:///croot/prompt-toolkit_1704404351921/work
psutil @ file:///opt/conda/conda-bld/psutil_1656431268089/work
ptyprocess @ file:///tmp/build/80754af9/ptyprocess_1609355006118/work/dist/ptyprocess-0.7.0-py2.py3-none-any.whl
pure-eval @ file:///opt/conda/conda-bld/pure_eval_1646925070566/work
pyarrow==15.0.2
pyarrow-hotfix==0.6
pycosat @ file:///croot/pycosat_1696536503704/work
pycparser @ file:///tmp/build/80754af9/pycparser_1636541352034/work
Pygments @ file:///croot/pygments_1684279966437/work
pyOpenSSL @ file:///croot/pyopenssl_1708380408460/work
PySocks @ file:///home/builder/ci_310/pysocks_1640793678128/work
python-dateutil==2.9.0.post0
python-etcd==0.4.5
pytz @ file:///croot/pytz_1695131579487/work
PyYAML @ file:///croot/pyyaml_1698096049011/work
referencing @ file:///croot/referencing_1699012038513/work
regex==2023.12.25
requests @ file:///croot/requests_1707355572290/work
rpds-py @ file:///croot/rpds-py_1698945930462/work
ruamel.yaml @ file:///croot/ruamel.yaml_1666304550667/work
ruamel.yaml.clib @ file:///croot/ruamel.yaml.clib_1666302247304/work
safetensors==0.4.2
scipy==1.13.0
six @ file:///tmp/build/80754af9/six_1644875935023/work
sortedcontainers==2.4.0
soupsieve @ file:///croot/soupsieve_1696347547217/work
stack-data @ file:///opt/conda/conda-bld/stack_data_1646927590127/work
sympy @ file:///croot/sympy_1701397643339/work
tokenizers==0.15.2
tomli @ file:///opt/conda/conda-bld/tomli_1657175507142/work
toolz @ file:///croot/toolz_1667464077321/work
torch==2.2.2
torchaudio==2.2.2
torchelastic==0.2.2
torchvision==0.17.2
tqdm @ file:///croot/tqdm_1679561862951/work
traitlets @ file:///croot/traitlets_1671143879854/work
transformers==4.39.3
triton==2.2.0
truststore @ file:///croot/truststore_1695244293384/work
types-dataclasses==0.6.6
typing_extensions==4.10.0
tzdata==2024.1
urllib3 @ file:///croot/urllib3_1707770551213/work
uv==0.1.29
wcwidth @ file:///Users/ktietz/demo/mc3/conda-bld/wcwidth_1629357192024/work
xxhash==3.4.1
yarl==1.9.4
zstandard @ file:///croot/zstandard_1677013143055/work
```

## Same using PIP:
```bash
> docker run -it pytorch/pytorch:2.2.2-cuda12.1-cudnn8-runtime bash
> pip install "torch>=2.2.0" datasets transformers peft "bitsandbytes==0.41.0" scipy
> pip freeze
accelerate==0.29.1
aiohttp==3.9.3
aiosignal==1.3.1
archspec @ file:///croot/archspec_1709217642129/work
asttokens @ file:///opt/conda/conda-bld/asttokens_1646925590279/work
astunparse==1.6.3
async-timeout==4.0.3
attrs @ file:///croot/attrs_1695717823297/work
beautifulsoup4 @ file:///croot/beautifulsoup4-split_1681493039619/work
bitsandbytes==0.41.0
boltons @ file:///croot/boltons_1677628692245/work
Brotli @ file:///tmp/abs_ecyw11_7ze/croots/recipe/brotli-split_1659616059936/work
certifi @ file:///croot/certifi_1707229174982/work/certifi
cffi @ file:///croot/cffi_1700254295673/work
chardet @ file:///home/builder/ci_310/chardet_1640804867535/work
charset-normalizer @ file:///tmp/build/80754af9/charset-normalizer_1630003229654/work
click @ file:///croot/click_1698129812380/work
conda @ file:///croot/conda_1696257509808/work
conda-build @ file:///croot/conda-build_1710789183177/work
conda-content-trust @ file:///croot/conda-content-trust_1693490622020/work
conda-libmamba-solver @ file:///croot/conda-libmamba-solver_1691418897561/work/src
conda-package-handling @ file:///croot/conda-package-handling_1690999929514/work
conda_index @ file:///croot/conda-index_1706633791028/work
conda_package_streaming @ file:///croot/conda-package-streaming_1690987966409/work
cryptography @ file:///croot/cryptography_1710350347627/work
datasets==2.18.0
decorator @ file:///opt/conda/conda-bld/decorator_1643638310831/work
dill==0.3.8
distro @ file:///croot/distro_1701455004953/work
dnspython==2.6.1
exceptiongroup @ file:///croot/exceptiongroup_1706031385326/work
executing @ file:///opt/conda/conda-bld/executing_1646925071911/work
expecttest==0.2.1
filelock @ file:///croot/filelock_1700591183607/work
frozenlist==1.4.1
fsspec==2024.2.0
gmpy2 @ file:///tmp/build/80754af9/gmpy2_1645455533097/work
huggingface-hub==0.22.2
hypothesis==6.99.13
idna @ file:///croot/idna_1666125576474/work
ipython @ file:///croot/ipython_1704833016303/work
jedi @ file:///tmp/build/80754af9/jedi_1644315229345/work
Jinja2 @ file:///croot/jinja2_1706733616596/work
jsonpatch @ file:///tmp/build/80754af9/jsonpatch_1615747632069/work
jsonpointer==2.1
jsonschema @ file:///croot/jsonschema_1699041609003/work
jsonschema-specifications @ file:///croot/jsonschema-specifications_1699032386549/work
libarchive-c @ file:///tmp/build/80754af9/python-libarchive-c_1617780486945/work
libmambapy @ file:///croot/mamba-split_1698782620632/work/libmambapy
MarkupSafe @ file:///croot/markupsafe_1704205993651/work
matplotlib-inline @ file:///opt/conda/conda-bld/matplotlib-inline_1662014470464/work
menuinst @ file:///croot/menuinst_1706732933928/work
mkl-fft @ file:///croot/mkl_fft_1695058164594/work
mkl-random @ file:///croot/mkl_random_1695059800811/work
mkl-service==2.4.0
more-itertools @ file:///croot/more-itertools_1700662129964/work
mpmath @ file:///croot/mpmath_1690848262763/work
multidict==6.0.5
multiprocess==0.70.16
networkx @ file:///croot/networkx_1690561992265/work
numpy @ file:///croot/numpy_and_numpy_base_1708638617955/work/dist/numpy-1.26.4-cp310-cp310-linux_x86_64.whl#sha256=d8cd837ed43e87f77e6efaa08e8de927ca030a1c9c5d04624432d6fb9a74a5ee
optree==0.11.0
packaging @ file:///croot/packaging_1710807400464/work
pandas==2.2.1
parso @ file:///opt/conda/conda-bld/parso_1641458642106/work
peft==0.10.0
pexpect @ file:///tmp/build/80754af9/pexpect_1605563209008/work
pillow @ file:///croot/pillow_1707233021655/work
pkginfo @ file:///croot/pkginfo_1679431160147/work
platformdirs @ file:///croot/platformdirs_1692205439124/work
pluggy @ file:///tmp/build/80754af9/pluggy_1648024709248/work
prompt-toolkit @ file:///croot/prompt-toolkit_1704404351921/work
psutil @ file:///opt/conda/conda-bld/psutil_1656431268089/work
ptyprocess @ file:///tmp/build/80754af9/ptyprocess_1609355006118/work/dist/ptyprocess-0.7.0-py2.py3-none-any.whl
pure-eval @ file:///opt/conda/conda-bld/pure_eval_1646925070566/work
pyarrow==15.0.2
pyarrow-hotfix==0.6
pycosat @ file:///croot/pycosat_1696536503704/work
pycparser @ file:///tmp/build/80754af9/pycparser_1636541352034/work
Pygments @ file:///croot/pygments_1684279966437/work
pyOpenSSL @ file:///croot/pyopenssl_1708380408460/work
PySocks @ file:///home/builder/ci_310/pysocks_1640793678128/work
python-dateutil==2.9.0.post0
python-etcd==0.4.5
pytz @ file:///croot/pytz_1695131579487/work
PyYAML @ file:///croot/pyyaml_1698096049011/work
referencing @ file:///croot/referencing_1699012038513/work
regex==2023.12.25
requests @ file:///croot/requests_1707355572290/work
rpds-py @ file:///croot/rpds-py_1698945930462/work
ruamel.yaml @ file:///croot/ruamel.yaml_1666304550667/work
ruamel.yaml.clib @ file:///croot/ruamel.yaml.clib_1666302247304/work
safetensors==0.4.2
scipy==1.13.0
six @ file:///tmp/build/80754af9/six_1644875935023/work
sortedcontainers==2.4.0
soupsieve @ file:///croot/soupsieve_1696347547217/work
stack-data @ file:///opt/conda/conda-bld/stack_data_1646927590127/work
sympy @ file:///croot/sympy_1701397643339/work
tokenizers==0.15.2
tomli @ file:///opt/conda/conda-bld/tomli_1657175507142/work
toolz @ file:///croot/toolz_1667464077321/work
torch==2.2.2
torchaudio==2.2.2
torchelastic==0.2.2
torchvision==0.17.2
tqdm @ file:///croot/tqdm_1679561862951/work
traitlets @ file:///croot/traitlets_1671143879854/work
transformers==4.39.3
triton==2.2.0
truststore @ file:///croot/truststore_1695244293384/work
types-dataclasses==0.6.6
typing_extensions==4.10.0
tzdata==2024.1
urllib3 @ file:///croot/urllib3_1707770551213/work
wcwidth @ file:///Users/ktietz/demo/mc3/conda-bld/wcwidth_1629357192024/work
xxhash==3.4.1
yarl==1.9.4
zstandard @ file:///croot/zstandard_1677013143055/work
```

I assume, it should be very straightforward to reproduce, given the base image, but would be more than happy to give you more info/logs, if needed!

Thank you!

---

_Referenced in [astral-sh/uv#2841](../../astral-sh/uv/issues/2841.md) on 2024-04-11 13:54_

---

_Comment by @samypr100 on 2024-04-12 01:56_

On your first example, what the output if you use `uv pip freeze` instead of `pip freeze`?

```diff
docker run -it pytorch/pytorch:2.2.2-cuda12.1-cudnn8-runtime bash
pip install uv
uv pip install "torch>=2.2.0" datasets transformers peft "bitsandbytes==0.41.0" scipy --verbose --system
-pip freeze
+uv pip freeze
```

---

_Comment by @avishniakov on 2024-04-12 05:14_

@samypr100 , if I do `uv pip freeze` the mentioned packages are visible, but still not usable from python scripts doing some CUDA-enabled codes. 
<details>
<summary>Full output of the `uv pip freeze`</summary>

```bash
> uv pip freeze
accelerate==0.29.2
aiohttp==3.9.4
aiosignal==1.3.1
archspec @ file:///croot/archspec_1709217642129/work
asttokens @ file:///opt/conda/conda-bld/asttokens_1646925590279/work
astunparse==1.6.3
async-timeout==4.0.3
attrs @ file:///croot/attrs_1695717823297/work
beautifulsoup4 @ file:///croot/beautifulsoup4-split_1681493039619/work
bitsandbytes==0.41.0
boltons @ file:///croot/boltons_1677628692245/work
brotli @ file:///tmp/abs_ecyw11_7ze/croots/recipe/brotli-split_1659616059936/work
certifi @ file:///croot/certifi_1707229174982/work/certifi
cffi @ file:///croot/cffi_1700254295673/work
chardet @ file:///home/builder/ci_310/chardet_1640804867535/work
charset-normalizer @ file:///tmp/build/80754af9/charset-normalizer_1630003229654/work
click @ file:///croot/click_1698129812380/work
conda @ file:///croot/conda_1696257509808/work
conda-build @ file:///croot/conda-build_1710789183177/work
conda-content-trust @ file:///croot/conda-content-trust_1693490622020/work
conda-index @ file:///croot/conda-index_1706633791028/work
conda-libmamba-solver @ file:///croot/conda-libmamba-solver_1691418897561/work/src
conda-package-handling @ file:///croot/conda-package-handling_1690999929514/work
conda-package-streaming @ file:///croot/conda-package-streaming_1690987966409/work
cryptography @ file:///croot/cryptography_1710350347627/work
datasets==2.18.0
decorator @ file:///opt/conda/conda-bld/decorator_1643638310831/work
dill==0.3.8
distro @ file:///croot/distro_1701455004953/work
dnspython==2.6.1
exceptiongroup @ file:///croot/exceptiongroup_1706031385326/work
executing @ file:///opt/conda/conda-bld/executing_1646925071911/work
expecttest==0.2.1
filelock @ file:///croot/filelock_1700591183607/work
frozenlist==1.4.1
fsspec==2024.2.0
gmpy2 @ file:///tmp/build/80754af9/gmpy2_1645455533097/work
huggingface-hub==0.22.2
hypothesis==6.99.13
idna @ file:///croot/idna_1666125576474/work
ipython @ file:///croot/ipython_1704833016303/work
jedi @ file:///tmp/build/80754af9/jedi_1644315229345/work
jinja2 @ file:///croot/jinja2_1706733616596/work
jsonpatch @ file:///tmp/build/80754af9/jsonpatch_1615747632069/work
jsonschema @ file:///croot/jsonschema_1699041609003/work
jsonschema-specifications @ file:///croot/jsonschema-specifications_1699032386549/work
libarchive-c @ file:///tmp/build/80754af9/python-libarchive-c_1617780486945/work
libmambapy @ file:///croot/mamba-split_1698782620632/work/libmambapy
markupsafe @ file:///croot/markupsafe_1704205993651/work
matplotlib-inline @ file:///opt/conda/conda-bld/matplotlib-inline_1662014470464/work
menuinst @ file:///croot/menuinst_1706732933928/work
mkl-fft @ file:///croot/mkl_fft_1695058164594/work
mkl-random @ file:///croot/mkl_random_1695059800811/work
more-itertools @ file:///croot/more-itertools_1700662129964/work
mpmath @ file:///croot/mpmath_1690848262763/work
multidict==6.0.5
multiprocess==0.70.16
networkx==3.2.1
numpy @ file:///croot/numpy_and_numpy_base_1708638617955/work/dist/numpy-1.26.4-cp310-cp310-linux_x86_64.whl
nvidia-cublas-cu12==12.1.3.1
nvidia-cuda-cupti-cu12==12.1.105
nvidia-cuda-nvrtc-cu12==12.1.105
nvidia-cuda-runtime-cu12==12.1.105
nvidia-cudnn-cu12==8.9.2.26
nvidia-cufft-cu12==11.0.2.54
nvidia-curand-cu12==10.3.2.106
nvidia-cusolver-cu12==11.4.5.107
nvidia-cusparse-cu12==12.1.0.106
nvidia-nccl-cu12==2.19.3
nvidia-nvjitlink-cu12==12.4.127
nvidia-nvtx-cu12==12.1.105
optree==0.11.0
packaging @ file:///croot/packaging_1710807400464/work
pandas==2.2.2
parso @ file:///opt/conda/conda-bld/parso_1641458642106/work
peft==0.10.0
pexpect @ file:///tmp/build/80754af9/pexpect_1605563209008/work
pillow @ file:///croot/pillow_1707233021655/work
pkginfo @ file:///croot/pkginfo_1679431160147/work
platformdirs @ file:///croot/platformdirs_1692205439124/work
pluggy @ file:///tmp/build/80754af9/pluggy_1648024709248/work
prompt-toolkit @ file:///croot/prompt-toolkit_1704404351921/work
psutil @ file:///opt/conda/conda-bld/psutil_1656431268089/work
ptyprocess @ file:///tmp/build/80754af9/ptyprocess_1609355006118/work/dist/ptyprocess-0.7.0-py2.py3-none-any.whl
pure-eval @ file:///opt/conda/conda-bld/pure_eval_1646925070566/work
pyarrow==15.0.2
pyarrow-hotfix==0.6
pycosat @ file:///croot/pycosat_1696536503704/work
pycparser @ file:///tmp/build/80754af9/pycparser_1636541352034/work
pygments @ file:///croot/pygments_1684279966437/work
pyopenssl @ file:///croot/pyopenssl_1708380408460/work
pysocks @ file:///home/builder/ci_310/pysocks_1640793678128/work
python-dateutil==2.9.0.post0
python-etcd==0.4.5
pytz @ file:///croot/pytz_1695131579487/work
pyyaml @ file:///croot/pyyaml_1698096049011/work
referencing @ file:///croot/referencing_1699012038513/work
regex==2023.12.25
requests @ file:///croot/requests_1707355572290/work
rpds-py @ file:///croot/rpds-py_1698945930462/work
ruamel-yaml @ file:///croot/ruamel.yaml_1666304550667/work
ruamel-yaml-clib @ file:///croot/ruamel.yaml.clib_1666302247304/work
safetensors==0.4.2
scipy==1.13.0
six @ file:///tmp/build/80754af9/six_1644875935023/work
sortedcontainers==2.4.0
soupsieve @ file:///croot/soupsieve_1696347547217/work
stack-data @ file:///opt/conda/conda-bld/stack_data_1646927590127/work
sympy @ file:///croot/sympy_1701397643339/work
tokenizers==0.15.2
tomli @ file:///opt/conda/conda-bld/tomli_1657175507142/work
toolz @ file:///croot/toolz_1667464077321/work
torch==2.2.2
torchelastic==0.2.2
tqdm @ file:///croot/tqdm_1679561862951/work
traitlets @ file:///croot/traitlets_1671143879854/work
transformers==4.39.3
triton==2.2.0
truststore @ file:///croot/truststore_1695244293384/work
types-dataclasses==0.6.6
typing-extensions==4.10.0
tzdata==2024.1
urllib3 @ file:///croot/urllib3_1707770551213/work
uv==0.1.31
wcwidth @ file:///Users/ktietz/demo/mc3/conda-bld/wcwidth_1629357192024/work
wheel==0.41.2
xxhash==3.4.1
yarl==1.9.4
zstandard @ file:///croot/zstandard_1677013143055/work
```

</details>

---

_Comment by @samypr100 on 2024-04-12 21:25_

Do you mind sharing some example code to allow reproducing the issue?

---

_Comment by @avishniakov on 2024-04-13 07:09_

Sure @samypr100 , if you run the following code inside a docker configured with `uv` as I described above you will hit an error. Important to note that you would need a proper CUDA GPU available in that docker, other wise you will see a `No GPU found.` error, which is nothing to do with the initially reported behaviour.

Code to reproduce:
```python
from transformers import AutoModelForCausalLM, BitsAndBytesConfig
import torch

bnb_config = BitsAndBytesConfig(
        load_in_8bit=True,
        bnb_4bit_use_double_quant=True,
        bnb_4bit_quant_type="nf4",
        bnb_4bit_compute_dtype=torch.bfloat16,
    )

model = AutoModelForCausalLM.from_pretrained(
    "mistralai/Mistral-7B-v0.1", quantization_config=bnb_config, device_map="auto"
)
```

Error you will see:
```
ImportError: Using `bitsandbytes` 8-bit quantization requires Accelerate: `pip install accelerate` and the latest version of bitsandbytes: `pip install -i https://pypi.org/simple/ bitsandbytes`
```

Before the error, there should be also a warning somewhere: `The installed version of bitsandbytes was compiled without GPU support.`

---

_Comment by @samypr100 on 2024-04-13 14:50_

On a clean `pytorch/pytorch:2.2.2-cuda12.1-cudnn8-runtime ` image (no uv, or anything), what's your output of

```
# python
>>> import torch
>>> torch.cuda.is_available()
True
```

Looking at the transformers code

```
>>>from transformers.utils.import_utils import is_torch_available, is_bitsandbytes_available, is_accelerate_available
>>>is_accelerate_available()
True
>>>is_torch_available()
True
>>>is_bitsandbytes_available()
True
```

`is_bitsandbytes_available` relies on `torch.cuda.is_available()` being true. Otherwise you get the message

```
ImportError: Using `bitsandbytes` 8-bit quantization requires Accelerate: `pip install accelerate` and the latest version of bitsandbytes: `pip install -i https://pypi.org/simple/ bitsandbytes`
```

---

_Comment by @samypr100 on 2024-04-13 15:00_

Are you missing `--gpus all` on your docker run command by any chance? 

---

_Comment by @avishniakov on 2024-04-13 15:21_

@samypr100 ,
On a clean setup I get `torch.cuda.is_available()` -> `True`.

The reason, why I gave my examples as just `docker run ...` without the `gpus` flag is the fact, that the GPUs are not available to the `docker build` and this is how we prepared the images for Vertex train jobs. But afterward when the Vertex training is running proper GPU flags are passed, they use the Nvidia Container Toolkit for that, afaik. The part about how images are spun up is out of our control, once we pass it down to Vertex, but there is a clear difference in how the `uv` built image behaves vs the `pip` built image, thus this issue was raised.

Hope this clarifies a bit the background and the context.
Let me know, if I can give you more info - would be happy to do it!

---

_Comment by @samypr100 on 2024-04-13 15:32_

Thanks, I think we'll likely need a more specific reproduceable example to figure out what the exact issue is. I gave your example a try and was able to run it with `uv` and with `pip`. Do note that without `--gpus all` it would fail on either `uv` or `pip` with the error you showed.

<img width="642" alt="run" src="https://github.com/astral-sh/uv/assets/3933065/920bdecc-0d74-4ba6-97ca-fd1696df6ad7">


---

_Comment by @avishniakov on 2024-04-13 15:44_

Thanks for checking, sure thing!

Here are 2 dockerfiles:
`pip.Dockerfile`
```docker
FROM pytorch/pytorch:2.2.2-cuda12.1-cudnn8-runtime
RUN pip install "torch>=2.2.0" datasets transformers peft "bitsandbytes==0.41.0" scipy
```

`uv.Dockerfile`
```docker
FROM pytorch/pytorch:2.2.2-cuda12.1-cudnn8-runtime
RUN pip install uv
RUN uv pip install "torch>=2.2.0" datasets transformers peft "bitsandbytes==0.41.0" scipy --verbose --system
```

Can you do:
1. `docker build -t uv-uv-issue . -f uv.Dockerfile`
2. `docker build -t pip-uv-issue . -f pip.Dockerfile`
3. `docker run -it --rm pip-uv-issue --gpus all bash` - run my example here, there should be no issues
4. `docker run -it --rm uv-uv-issue --gpus all bash` - run my example here - you should see the error


I bet that it worked for you because you did a docker run with GPUs and then installed libs it inside, but as I said no GPUs will be available on build.

---

_Comment by @samypr100 on 2024-04-13 16:42_

Thanks

I was able to reproduce. 

The core issue is that the original image had its pre-installed dependencies built with gpu support (gpu's were attached at built time).
When `uv` ends up reinstalling some of them (and given that you don't have gpu's attached at build time), the produced build and linking process ends up being inherently different. This causes the issues later when you try to run with `--gpus all` since some of those dependencies are not built with gpu support anymore.

This is definitely related to https://github.com/astral-sh/uv/issues/2841 as @StellarStorm pointed out and would only be resolved if `uv` didn't re-install these system site packages (e.g. `torch`, `nvidia-*`) when they're already pre-installed.

Some follow on investigation is still needed to determine why `uv` is reinstalling them.

---

_Comment by @charliermarsh on 2024-04-13 16:50_

Thanks @samypr100. Do you know _where_ they're installed? Maybe on `sys.path` but not in any of the `site-packages` directories?

---

_Comment by @samypr100 on 2024-04-13 18:36_

Noticed that `torch` `torchaudio` and `torchvision` are pre-installed as eggs which might explain the root of the problem.

Here's the PKG-INFO for torch (/opt/conda/lib/python3.10/site-packages/torch-2.2.2-py3.10.egg-info/PKG-INFO)

<details>

```
Metadata-Version: 2.1
Name: torch
Version: 2.2.2
Summary: Tensors and Dynamic neural networks in Python with strong GPU acceleration
Home-page: https://pytorch.org/
Download-URL: https://github.com/pytorch/pytorch/tags
Author: PyTorch Team
Author-email: packages@pytorch.org
License: BSD-3
Keywords: pytorch,machine learning
Classifier: Development Status :: 5 - Production/Stable
Classifier: Intended Audience :: Developers
Classifier: Intended Audience :: Education
Classifier: Intended Audience :: Science/Research
Classifier: License :: OSI Approved :: BSD License
Classifier: Topic :: Scientific/Engineering
Classifier: Topic :: Scientific/Engineering :: Mathematics
Classifier: Topic :: Scientific/Engineering :: Artificial Intelligence
Classifier: Topic :: Software Development
Classifier: Topic :: Software Development :: Libraries
Classifier: Topic :: Software Development :: Libraries :: Python Modules
Classifier: Programming Language :: C++
Classifier: Programming Language :: Python :: 3
Classifier: Programming Language :: Python :: 3.8
Classifier: Programming Language :: Python :: 3.9
Classifier: Programming Language :: Python :: 3.10
Classifier: Programming Language :: Python :: 3.11
Classifier: Programming Language :: Python :: 3.12
Requires-Python: >=3.8.0
Description-Content-Type: text/markdown
License-File: LICENSE
License-File: NOTICE
Requires-Dist: filelock
Requires-Dist: typing-extensions>=4.8.0
Requires-Dist: sympy
Requires-Dist: networkx
Requires-Dist: jinja2
Requires-Dist: fsspec
Provides-Extra: optree
Requires-Dist: optree>=0.9.1; extra == "optree"
Provides-Extra: opt-einsum
Requires-Dist: opt-einsum>=3.3; extra == "opt-einsum"
```

</details>

Here's all the eggs in site-packages `/opt/conda/lib/python3.10/site-packages`
```
jsonpointer-2.1-py3.6.egg-info
mkl_service-2.4.0-py3.10.egg-info
pip-23.3.1-py3.10.egg-info
setuptools-68.2.2-py3.10.egg-info
torch-2.2.2-py3.10.egg-info
torchaudio-2.2.2-py3.10.egg-info
torchvision-0.17.2-py3.10.egg-info
triton-2.2.0-py3.10.egg-info
```

For example, after uv re-installs "torch>=2.2.0", you get all the proper .dist-info versions

```
torch-2.2.2.dist-info
nvidia_cublas_cu12-12.1.3.1.dist-info
nvidia_cuda_cupti_cu12-12.1.105.dist-info
nvidia_cuda_nvrtc_cu12-12.1.105.dist-info
nvidia_cuda_runtime_cu12-12.1.105.dist-info
nvidia_cudnn_cu12-8.9.2.26.dist-info
nvidia_cufft_cu12-11.0.2.54.dist-info
nvidia_curand_cu12-10.3.2.106.dist-info
nvidia_cusolver_cu12-11.4.5.107.dist-info
nvidia_cusparse_cu12-12.1.0.106.dist-info
nvidia_nccl_cu12-2.19.3.dist-info
nvidia_nvjitlink_cu12-12.4.127.dist-info
nvidia_nvtx_cu12-12.1.105.dist-info
```

and here's the contents of `METADATA` for torch-2.2.2 (which has the `Requires-Dist` for nvidia dependencies)

<details>

```
Metadata-Version: 2.1
Name: torch
Version: 2.2.2
Summary: Tensors and Dynamic neural networks in Python with strong GPU acceleration
Home-page: https://pytorch.org/
Author: PyTorch Team
Author-email: packages@pytorch.org
License: BSD-3
Download-URL: https://github.com/pytorch/pytorch/tags
Keywords: pytorch,machine learning
Platform: UNKNOWN
Classifier: Development Status :: 5 - Production/Stable
Classifier: Intended Audience :: Developers
Classifier: Intended Audience :: Education
Classifier: Intended Audience :: Science/Research
Classifier: License :: OSI Approved :: BSD License
Classifier: Topic :: Scientific/Engineering
Classifier: Topic :: Scientific/Engineering :: Mathematics
Classifier: Topic :: Scientific/Engineering :: Artificial Intelligence
Classifier: Topic :: Software Development
Classifier: Topic :: Software Development :: Libraries
Classifier: Topic :: Software Development :: Libraries :: Python Modules
Classifier: Programming Language :: C++
Classifier: Programming Language :: Python :: 3
Classifier: Programming Language :: Python :: 3.8
Classifier: Programming Language :: Python :: 3.9
Classifier: Programming Language :: Python :: 3.10
Classifier: Programming Language :: Python :: 3.11
Classifier: Programming Language :: Python :: 3.12
Requires-Python: >=3.8.0
Description-Content-Type: text/markdown
License-File: LICENSE
License-File: NOTICE
Requires-Dist: filelock
Requires-Dist: typing-extensions (>=4.8.0)
Requires-Dist: sympy
Requires-Dist: networkx
Requires-Dist: jinja2
Requires-Dist: fsspec
Requires-Dist: nvidia-cuda-nvrtc-cu12 (==12.1.105) ; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cuda-runtime-cu12 (==12.1.105) ; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cuda-cupti-cu12 (==12.1.105) ; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cudnn-cu12 (==8.9.2.26) ; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cublas-cu12 (==12.1.3.1) ; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cufft-cu12 (==11.0.2.54) ; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-curand-cu12 (==10.3.2.106) ; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cusolver-cu12 (==11.4.5.107) ; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-cusparse-cu12 (==12.1.0.106) ; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-nccl-cu12 (==2.19.3) ; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: nvidia-nvtx-cu12 (==12.1.105) ; platform_system == "Linux" and platform_machine == "x86_64"
Requires-Dist: triton (==2.2.0) ; platform_system == "Linux" and platform_machine == "x86_64" and python_version < "3.12"
Provides-Extra: opt-einsum
Requires-Dist: opt-einsum (>=3.3) ; extra == 'opt-einsum'
Provides-Extra: optree
Requires-Dist: optree (>=0.9.1) ; extra == 'optree'
```

</details>

Overall, seems like it still installs them in `/opt/conda/lib/python3.10/site-packages` as expected so no issues there, below output for sys.path seems pretty standard, except that everything is prefixed with `/opt/conda` instead of the traditional locations, e.g. `/usr/local`

```
>>> sys.path
['', '/opt/conda/lib/python310.zip', '/opt/conda/lib/python3.10', '/opt/conda/lib/python3.10/lib-dynload', '/opt/conda/lib/python3.10/site-packages']
>>> site.getsitepackages()
['/opt/conda/lib/python3.10/site-packages']
>>> site.getusersitepackages()
'/root/.local/lib/python3.10/site-packages'
```

Here's the output of `get_interpreter_info` on this image -everything seems prefixed correctly to `/opt/conda` from what I can tell.

<details>

```json
{
    "result": "success",
    "markers": {
        "implementation_name": "cpython",
        "implementation_version": "3.10.14",
        "os_name": "posix",
        "platform_machine": "x86_64",
        "platform_python_implementation": "CPython",
        "platform_release": "5.15.146.1-microsoft-standard-WSL2",
        "platform_system": "Linux",
        "platform_version": "#1 SMP Thu Jan 11 04:09:03 UTC 2024",
        "python_full_version": "3.10.14",
        "python_version": "3.10",
        "sys_platform": "linux"
    },
    "base_prefix": "/opt/conda",
    "base_exec_prefix": "/opt/conda",
    "prefix": "/opt/conda",
    "base_executable": "/opt/conda/bin/python",
    "sys_executable": "/opt/conda/bin/python",
    "stdlib": "/opt/conda/lib/python3.10",
    "scheme": {
        "platlib": "/opt/conda/lib/python3.10/site-packages",
        "purelib": "/opt/conda/lib/python3.10/site-packages",
        "include": "/opt/conda/include/python3.10",
        "scripts": "/opt/conda/bin",
        "data": "/opt/conda"
    },
    "virtualenv": {
        "purelib": "lib/python3.10/site-packages",
        "platlib": "lib/python3.10/site-packages",
        "include": "include/site/python3.10",
        "scripts": "bin",
        "data": ""
    },
    "platform": {
        "os": {
            "name": "manylinux",
            "major": 2,
            "minor": 35
        },
        "arch": "x86_64"
    },
    "gil_disabled": false
}
```

</details>


---

_Comment by @charliermarsh on 2024-04-13 18:37_

Ah interesting. Yeah we don't support eggs right now. We weren't planning on supporting them at all. (I don't know any of the details on how the format works.)

---

_Comment by @samypr100 on 2024-04-13 18:39_

`torchelastic` is not preinstalled an egg, which explains https://github.com/astral-sh/uv/issues/2841

---

_Comment by @samypr100 on 2024-04-13 18:41_

 @StellarStorm @avishniakov The one alternative I can think of is seeing if upstream maintainers for `pytorch/pytorch:2.2.2-cuda12.1-cudnn8-runtime`, and related images can update them to avoid using egg installs. That should make the images uv compatible.

---

_Comment by @charliermarsh on 2024-04-13 18:43_

I can briefly look into what it would take to support pre-installed eggs. (I.e., recognizing that they’re present, and knowing how to uninstall them if they need to be upgraded.)

---

_Comment by @charliermarsh on 2024-04-13 18:54_

I think it could make sense to _at least_ support detecting that eggs are installed.


---

_Comment by @samypr100 on 2024-04-13 18:56_

Makes sense to at least detect and maybe issue a warning.
With the deprecation of eggs via [PEP-0715](https://peps.python.org/pep-0715/), we'll hopefully have less eggs and more wheels in the future.

---

_Comment by @samypr100 on 2024-04-13 19:10_

> @StellarStorm @avishniakov The one alternative I can think of is seeing if upstream maintainers for pytorch/pytorch:2.2.2-cuda12.1-cudnn8-runtime, and related images can update them to avoid using egg installs. That should make the images uv compatible.

It's possible a small update to https://github.com/pytorch/pytorch/blob/main/Dockerfile to use `python -m pip install .` instead of `python setup.py install` might do the trick.

---

_Comment by @charliermarsh on 2024-04-13 21:46_

Uninstalling them looks a little tricky though. I can't even figure out how to build and install an egg locally since `easy_install` is long deprecated.

---

_Comment by @charliermarsh on 2024-04-13 21:54_

I figured out how to build and install a `.egg`, but any idea how you get a `.egg-info` as above...?

---

_Comment by @samypr100 on 2024-04-13 23:28_

The best way I know of is making a dummy python project with a setup.py and running `python setup.py egg_info` or `python setup.py install`

Given a dummy `egg_test` python module/project

```
egg_test
 ├── setup.py
 └── egg_test
      └── __init__.py # empty
```

and setup.py having this

```python
from setuptools import setup, find_packages

setup(
    name='egg_test',
    version='0.1',
    description='Lorem Ipsum',
    long_description='Lorem Ipsum',
    author='Lorem Ipsum',
    author_email='lorem.ipsum@example.com',
    packages=find_packages(),
    install_requires=[
        "flask"
    ],
    classifiers=[
        'License :: OSI Approved :: MIT License',
        'Programming Language :: Python :: 3'
        'Programming Language :: Python :: 3.11'
    ],
)
```

You can run `python setup.py egg_info` to see what .egg_info structure looks like. You may need to install `setuptools`.


---

_Comment by @samypr100 on 2024-04-13 23:35_

Seems like the implementation for generating .egg-info directories is here https://github.com/pypa/setuptools/blob/main/setuptools/command/egg_info.py
and general info is available here https://setuptools.pypa.io/en/latest/deprecated/python_eggs.html

---

_Referenced in [pytorch/pytorch#124027](../../pytorch/pytorch/pulls/124027.md) on 2024-04-14 23:48_

---

_Referenced in [astral-sh/uv#3341](../../astral-sh/uv/issues/3341.md) on 2024-05-03 22:06_

---

_Referenced in [astral-sh/uv#3380](../../astral-sh/uv/pulls/3380.md) on 2024-05-05 23:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-05 23:20_

---

_Comment by @charliermarsh on 2024-05-05 23:20_

Put up a PR to fix this: https://github.com/astral-sh/uv/pull/3380

---

_Closed by @charliermarsh on 2024-05-06 13:47_

---

_Comment by @strickvl on 2024-05-06 13:48_

Thank you!

---

_Comment by @samypr100 on 2024-05-08 00:20_

@avishniakov Would you be able to confirm 0.1.40 fixes your reported issue?

---

_Comment by @avishniakov on 2024-05-14 07:10_

> @avishniakov Would you be able to confirm 0.1.40 fixes your reported issue?

Hey @samypr100 , sorry about the delay! I checked that in the original settings as it was reported and the issue is resolved now. Thanks for your support!

---
