```yaml
number: 7516
title: Pyarrow wheel installation failure
type: issue
state: closed
author: jimmy-marmalade
labels:
  - needs-mre
assignees: []
created_at: 2024-09-18T19:45:08Z
updated_at: 2025-09-22T04:21:50Z
url: https://github.com/astral-sh/uv/issues/7516
synced_at: 2026-01-10T03:23:52Z
```

# Pyarrow wheel installation failure

---

_Issue opened by @jimmy-marmalade on 2024-09-18 19:45_

Hi, I'm on

```
uv --version
uv 0.4.12 (2545bca69 2024-09-18)
```

installation of pyarrow wheel fails:

```
uv pip install -v ~/Downloads/pyarrow-17.0.0-cp312-cp312-macosx_11_0_arm64.whl
DEBUG uv 0.4.12
DEBUG Searching for Python interpreter in system path
DEBUG Found `cpython-3.12.6-macos-aarch64-none` at `/Users/jimmy/dev/monolith/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.6 environment at .venv/bin/python3
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: file:///Users/jimmy/Downloads/pyarrow-17.0.0-cp312-cp312-macosx_11_0_arm64.whl
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.6
DEBUG Solving with target Python version: >=3.12.6
DEBUG Adding direct dependency: pyarrow*
DEBUG Searching for a compatible version of pyarrow @ file:///Users/jimmy/Downloads/pyarrow-17.0.0-cp312-cp312-macosx_11_0_arm64.whl (*)
DEBUG Adding transitive dependency for pyarrow==17.0.0: numpy>=1.16.6
DEBUG Found fresh response for: https://pypi.org/simple/numpy/
DEBUG Searching for a compatible version of numpy (>=1.16.6)
DEBUG Selecting: numpy==2.1.1 [compatible] (numpy-2.1.1.tar.gz)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6b/9e/8bc6f133bc6d359ccc9ec051853aded45504d217685191f31f46d36b7065/numpy-2.1.1-cp313-cp313-macosx_10_13_x86_64.whl.metadata
DEBUG Tried 2 versions: numpy 1, pyarrow 1
DEBUG Split specific environment resolution took 0.005s
Resolved 2 packages in 49ms
DEBUG Identified uncached requirement: numpy==2.1.1
DEBUG Released lock at `/Users/jimmy/dev/monolith/.venv/.lock`
error: Failed to determine installation plan
  Caused by: A path dependency is incompatible with the current platform: /Users/jimmy/Downloads/pyarrow-17.0.0-cp312-cp312-macosx_11_0_arm64.whl
```

while platform does appear to be supported by pip:

```
python -m pip debug -v
WARNING: This command is only meant for debugging. Do not use this with automation for parsing and getting these details, since the output and options of this command may change without notice.
pip version: pip 24.2 from /Users/jimmy/.pyenv/versions/3.12.6/lib/python3.12/site-packages/pip (python 3.12)
sys.version: 3.12.6 (main, Sep 18 2024, 14:18:06) [Clang 15.0.0 (clang-1500.1.0.2.5)]
sys.executable: /Users/jimmy/.pyenv/versions/3.12.6/bin/python
sys.getdefaultencoding: utf-8
sys.getfilesystemencoding: utf-8
locale.getpreferredencoding: UTF-8
sys.platform: darwin
sys.implementation:
  name: cpython
'cert' config value: Not specified
REQUESTS_CA_BUNDLE: None
CURL_CA_BUNDLE: None
pip._vendor.certifi.where(): /Users/jimmy/.pyenv/versions/3.12.6/lib/python3.12/site-packages/pip/_vendor/certifi/cacert.pem
pip._vendor.DEBUNDLED: False
vendored library versions:
  CacheControl==0.14.0
  distlib==0.3.8
  distro==1.9.0
  msgpack==1.0.8
  packaging==24.1
  platformdirs==4.2.2
  pyproject-hooks==1.0.0
  requests==2.32.3
  certifi==2024.07.04
  idna==3.7
  urllib3==1.26.18
  rich==13.7.1 (Unable to locate actual module version, using vendor.txt specified version)
  pygments==2.18.0
  typing_extensions==4.12.2 (Unable to locate actual module version, using vendor.txt specified version)
  resolvelib==1.0.1
  setuptools==70.3.0 (Unable to locate actual module version, using vendor.txt specified version)
  tomli==2.0.1
  truststore==0.9.1
Compatible tags: 582
  cp312-cp312-macosx_14_0_arm64
  cp312-cp312-macosx_14_0_universal2
  cp312-cp312-macosx_13_0_arm64
  cp312-cp312-macosx_13_0_universal2
  cp312-cp312-macosx_12_0_arm64
  cp312-cp312-macosx_12_0_universal2
  cp312-cp312-macosx_11_0_arm64
  cp312-cp312-macosx_11_0_universal2
  cp312-cp312-macosx_10_16_universal2
  cp312-cp312-macosx_10_15_universal2
  cp312-cp312-macosx_10_14_universal2
  cp312-cp312-macosx_10_13_universal2
  cp312-cp312-macosx_10_12_universal2
  cp312-cp312-macosx_10_11_universal2
  cp312-cp312-macosx_10_10_universal2
  cp312-cp312-macosx_10_9_universal2
  cp312-cp312-macosx_10_8_universal2
  cp312-cp312-macosx_10_7_universal2
  cp312-cp312-macosx_10_6_universal2
  cp312-cp312-macosx_10_5_universal2
  cp312-cp312-macosx_10_4_universal2
  cp312-abi3-macosx_14_0_arm64
  cp312-abi3-macosx_14_0_universal2
  cp312-abi3-macosx_13_0_arm64
  cp312-abi3-macosx_13_0_universal2
  cp312-abi3-macosx_12_0_arm64
  cp312-abi3-macosx_12_0_universal2
  cp312-abi3-macosx_11_0_arm64
  cp312-abi3-macosx_11_0_universal2
  cp312-abi3-macosx_10_16_universal2
  cp312-abi3-macosx_10_15_universal2
  cp312-abi3-macosx_10_14_universal2
  cp312-abi3-macosx_10_13_universal2
  cp312-abi3-macosx_10_12_universal2
  cp312-abi3-macosx_10_11_universal2
  cp312-abi3-macosx_10_10_universal2
  cp312-abi3-macosx_10_9_universal2
  cp312-abi3-macosx_10_8_universal2
  cp312-abi3-macosx_10_7_universal2
  cp312-abi3-macosx_10_6_universal2
  cp312-abi3-macosx_10_5_universal2
  cp312-abi3-macosx_10_4_universal2
  cp312-none-macosx_14_0_arm64
  cp312-none-macosx_14_0_universal2
  cp312-none-macosx_13_0_arm64
  cp312-none-macosx_13_0_universal2
  cp312-none-macosx_12_0_arm64
  cp312-none-macosx_12_0_universal2
  cp312-none-macosx_11_0_arm64
  cp312-none-macosx_11_0_universal2
  cp312-none-macosx_10_16_universal2
  cp312-none-macosx_10_15_universal2
  cp312-none-macosx_10_14_universal2
  cp312-none-macosx_10_13_universal2
  cp312-none-macosx_10_12_universal2
  cp312-none-macosx_10_11_universal2
  cp312-none-macosx_10_10_universal2
  cp312-none-macosx_10_9_universal2
  cp312-none-macosx_10_8_universal2
  cp312-none-macosx_10_7_universal2
  cp312-none-macosx_10_6_universal2
  cp312-none-macosx_10_5_universal2
  cp312-none-macosx_10_4_universal2
  cp311-abi3-macosx_14_0_arm64
  cp311-abi3-macosx_14_0_universal2
  cp311-abi3-macosx_13_0_arm64
  cp311-abi3-macosx_13_0_universal2
  cp311-abi3-macosx_12_0_arm64
  cp311-abi3-macosx_12_0_universal2
  cp311-abi3-macosx_11_0_arm64
  cp311-abi3-macosx_11_0_universal2
  cp311-abi3-macosx_10_16_universal2
  cp311-abi3-macosx_10_15_universal2
  cp311-abi3-macosx_10_14_universal2
  cp311-abi3-macosx_10_13_universal2
  cp311-abi3-macosx_10_12_universal2
  cp311-abi3-macosx_10_11_universal2
  cp311-abi3-macosx_10_10_universal2
  cp311-abi3-macosx_10_9_universal2
  cp311-abi3-macosx_10_8_universal2
  cp311-abi3-macosx_10_7_universal2
  cp311-abi3-macosx_10_6_universal2
  cp311-abi3-macosx_10_5_universal2
  cp311-abi3-macosx_10_4_universal2
  cp310-abi3-macosx_14_0_arm64
  cp310-abi3-macosx_14_0_universal2
  cp310-abi3-macosx_13_0_arm64
  cp310-abi3-macosx_13_0_universal2
  cp310-abi3-macosx_12_0_arm64
  cp310-abi3-macosx_12_0_universal2
  cp310-abi3-macosx_11_0_arm64
  cp310-abi3-macosx_11_0_universal2
  cp310-abi3-macosx_10_16_universal2
  cp310-abi3-macosx_10_15_universal2
  cp310-abi3-macosx_10_14_universal2
  cp310-abi3-macosx_10_13_universal2
  cp310-abi3-macosx_10_12_universal2
  cp310-abi3-macosx_10_11_universal2
  cp310-abi3-macosx_10_10_universal2
  cp310-abi3-macosx_10_9_universal2
  cp310-abi3-macosx_10_8_universal2
  cp310-abi3-macosx_10_7_universal2
  cp310-abi3-macosx_10_6_universal2
  cp310-abi3-macosx_10_5_universal2
  cp310-abi3-macosx_10_4_universal2
  cp39-abi3-macosx_14_0_arm64
  cp39-abi3-macosx_14_0_universal2
  cp39-abi3-macosx_13_0_arm64
  cp39-abi3-macosx_13_0_universal2
  cp39-abi3-macosx_12_0_arm64
  cp39-abi3-macosx_12_0_universal2
  cp39-abi3-macosx_11_0_arm64
  cp39-abi3-macosx_11_0_universal2
  cp39-abi3-macosx_10_16_universal2
  cp39-abi3-macosx_10_15_universal2
  cp39-abi3-macosx_10_14_universal2
  cp39-abi3-macosx_10_13_universal2
  cp39-abi3-macosx_10_12_universal2
  cp39-abi3-macosx_10_11_universal2
  cp39-abi3-macosx_10_10_universal2
  cp39-abi3-macosx_10_9_universal2
  cp39-abi3-macosx_10_8_universal2
  cp39-abi3-macosx_10_7_universal2
  cp39-abi3-macosx_10_6_universal2
  cp39-abi3-macosx_10_5_universal2
  cp39-abi3-macosx_10_4_universal2
  cp38-abi3-macosx_14_0_arm64
  cp38-abi3-macosx_14_0_universal2
  cp38-abi3-macosx_13_0_arm64
  cp38-abi3-macosx_13_0_universal2
  cp38-abi3-macosx_12_0_arm64
  cp38-abi3-macosx_12_0_universal2
  cp38-abi3-macosx_11_0_arm64
  cp38-abi3-macosx_11_0_universal2
  cp38-abi3-macosx_10_16_universal2
  cp38-abi3-macosx_10_15_universal2
  cp38-abi3-macosx_10_14_universal2
  cp38-abi3-macosx_10_13_universal2
  cp38-abi3-macosx_10_12_universal2
  cp38-abi3-macosx_10_11_universal2
  cp38-abi3-macosx_10_10_universal2
  cp38-abi3-macosx_10_9_universal2
  cp38-abi3-macosx_10_8_universal2
  cp38-abi3-macosx_10_7_universal2
  cp38-abi3-macosx_10_6_universal2
  cp38-abi3-macosx_10_5_universal2
  cp38-abi3-macosx_10_4_universal2
  cp37-abi3-macosx_14_0_arm64
  cp37-abi3-macosx_14_0_universal2
  cp37-abi3-macosx_13_0_arm64
  cp37-abi3-macosx_13_0_universal2
  cp37-abi3-macosx_12_0_arm64
  cp37-abi3-macosx_12_0_universal2
  cp37-abi3-macosx_11_0_arm64
  cp37-abi3-macosx_11_0_universal2
  cp37-abi3-macosx_10_16_universal2
  cp37-abi3-macosx_10_15_universal2
  cp37-abi3-macosx_10_14_universal2
  cp37-abi3-macosx_10_13_universal2
  cp37-abi3-macosx_10_12_universal2
  cp37-abi3-macosx_10_11_universal2
  cp37-abi3-macosx_10_10_universal2
  cp37-abi3-macosx_10_9_universal2
  cp37-abi3-macosx_10_8_universal2
  cp37-abi3-macosx_10_7_universal2
  cp37-abi3-macosx_10_6_universal2
  cp37-abi3-macosx_10_5_universal2
  cp37-abi3-macosx_10_4_universal2
  cp36-abi3-macosx_14_0_arm64
  cp36-abi3-macosx_14_0_universal2
  cp36-abi3-macosx_13_0_arm64
  cp36-abi3-macosx_13_0_universal2
  cp36-abi3-macosx_12_0_arm64
  cp36-abi3-macosx_12_0_universal2
  cp36-abi3-macosx_11_0_arm64
  cp36-abi3-macosx_11_0_universal2
  cp36-abi3-macosx_10_16_universal2
  cp36-abi3-macosx_10_15_universal2
  cp36-abi3-macosx_10_14_universal2
  cp36-abi3-macosx_10_13_universal2
  cp36-abi3-macosx_10_12_universal2
  cp36-abi3-macosx_10_11_universal2
  cp36-abi3-macosx_10_10_universal2
  cp36-abi3-macosx_10_9_universal2
  cp36-abi3-macosx_10_8_universal2
  cp36-abi3-macosx_10_7_universal2
  cp36-abi3-macosx_10_6_universal2
  cp36-abi3-macosx_10_5_universal2
  cp36-abi3-macosx_10_4_universal2
  cp35-abi3-macosx_14_0_arm64
  cp35-abi3-macosx_14_0_universal2
  cp35-abi3-macosx_13_0_arm64
  cp35-abi3-macosx_13_0_universal2
  cp35-abi3-macosx_12_0_arm64
  cp35-abi3-macosx_12_0_universal2
  cp35-abi3-macosx_11_0_arm64
  cp35-abi3-macosx_11_0_universal2
  cp35-abi3-macosx_10_16_universal2
  cp35-abi3-macosx_10_15_universal2
  cp35-abi3-macosx_10_14_universal2
  cp35-abi3-macosx_10_13_universal2
  cp35-abi3-macosx_10_12_universal2
  cp35-abi3-macosx_10_11_universal2
  cp35-abi3-macosx_10_10_universal2
  cp35-abi3-macosx_10_9_universal2
  cp35-abi3-macosx_10_8_universal2
  cp35-abi3-macosx_10_7_universal2
  cp35-abi3-macosx_10_6_universal2
  cp35-abi3-macosx_10_5_universal2
  cp35-abi3-macosx_10_4_universal2
  cp34-abi3-macosx_14_0_arm64
  cp34-abi3-macosx_14_0_universal2
  cp34-abi3-macosx_13_0_arm64
  cp34-abi3-macosx_13_0_universal2
  cp34-abi3-macosx_12_0_arm64
  cp34-abi3-macosx_12_0_universal2
  cp34-abi3-macosx_11_0_arm64
  cp34-abi3-macosx_11_0_universal2
  cp34-abi3-macosx_10_16_universal2
  cp34-abi3-macosx_10_15_universal2
  cp34-abi3-macosx_10_14_universal2
  cp34-abi3-macosx_10_13_universal2
  cp34-abi3-macosx_10_12_universal2
  cp34-abi3-macosx_10_11_universal2
  cp34-abi3-macosx_10_10_universal2
  cp34-abi3-macosx_10_9_universal2
  cp34-abi3-macosx_10_8_universal2
  cp34-abi3-macosx_10_7_universal2
  cp34-abi3-macosx_10_6_universal2
  cp34-abi3-macosx_10_5_universal2
  cp34-abi3-macosx_10_4_universal2
  cp33-abi3-macosx_14_0_arm64
  cp33-abi3-macosx_14_0_universal2
  cp33-abi3-macosx_13_0_arm64
  cp33-abi3-macosx_13_0_universal2
  cp33-abi3-macosx_12_0_arm64
  cp33-abi3-macosx_12_0_universal2
  cp33-abi3-macosx_11_0_arm64
  cp33-abi3-macosx_11_0_universal2
  cp33-abi3-macosx_10_16_universal2
  cp33-abi3-macosx_10_15_universal2
  cp33-abi3-macosx_10_14_universal2
  cp33-abi3-macosx_10_13_universal2
  cp33-abi3-macosx_10_12_universal2
  cp33-abi3-macosx_10_11_universal2
  cp33-abi3-macosx_10_10_universal2
  cp33-abi3-macosx_10_9_universal2
  cp33-abi3-macosx_10_8_universal2
  cp33-abi3-macosx_10_7_universal2
  cp33-abi3-macosx_10_6_universal2
  cp33-abi3-macosx_10_5_universal2
  cp33-abi3-macosx_10_4_universal2
  cp32-abi3-macosx_14_0_arm64
  cp32-abi3-macosx_14_0_universal2
  cp32-abi3-macosx_13_0_arm64
  cp32-abi3-macosx_13_0_universal2
  cp32-abi3-macosx_12_0_arm64
  cp32-abi3-macosx_12_0_universal2
  cp32-abi3-macosx_11_0_arm64
  cp32-abi3-macosx_11_0_universal2
  cp32-abi3-macosx_10_16_universal2
  cp32-abi3-macosx_10_15_universal2
  cp32-abi3-macosx_10_14_universal2
  cp32-abi3-macosx_10_13_universal2
  cp32-abi3-macosx_10_12_universal2
  cp32-abi3-macosx_10_11_universal2
  cp32-abi3-macosx_10_10_universal2
  cp32-abi3-macosx_10_9_universal2
  cp32-abi3-macosx_10_8_universal2
  cp32-abi3-macosx_10_7_universal2
  cp32-abi3-macosx_10_6_universal2
  cp32-abi3-macosx_10_5_universal2
  cp32-abi3-macosx_10_4_universal2
  py312-none-macosx_14_0_arm64
  py312-none-macosx_14_0_universal2
  py312-none-macosx_13_0_arm64
  py312-none-macosx_13_0_universal2
  py312-none-macosx_12_0_arm64
  py312-none-macosx_12_0_universal2
  py312-none-macosx_11_0_arm64
  py312-none-macosx_11_0_universal2
  py312-none-macosx_10_16_universal2
  py312-none-macosx_10_15_universal2
  py312-none-macosx_10_14_universal2
  py312-none-macosx_10_13_universal2
  py312-none-macosx_10_12_universal2
  py312-none-macosx_10_11_universal2
  py312-none-macosx_10_10_universal2
  py312-none-macosx_10_9_universal2
  py312-none-macosx_10_8_universal2
  py312-none-macosx_10_7_universal2
  py312-none-macosx_10_6_universal2
  py312-none-macosx_10_5_universal2
  py312-none-macosx_10_4_universal2
  py3-none-macosx_14_0_arm64
  py3-none-macosx_14_0_universal2
  py3-none-macosx_13_0_arm64
  py3-none-macosx_13_0_universal2
  py3-none-macosx_12_0_arm64
  py3-none-macosx_12_0_universal2
  py3-none-macosx_11_0_arm64
  py3-none-macosx_11_0_universal2
  py3-none-macosx_10_16_universal2
  py3-none-macosx_10_15_universal2
  py3-none-macosx_10_14_universal2
  py3-none-macosx_10_13_universal2
  py3-none-macosx_10_12_universal2
  py3-none-macosx_10_11_universal2
  py3-none-macosx_10_10_universal2
  py3-none-macosx_10_9_universal2
  py3-none-macosx_10_8_universal2
  py3-none-macosx_10_7_universal2
  py3-none-macosx_10_6_universal2
  py3-none-macosx_10_5_universal2
  py3-none-macosx_10_4_universal2
  py311-none-macosx_14_0_arm64
  py311-none-macosx_14_0_universal2
  py311-none-macosx_13_0_arm64
  py311-none-macosx_13_0_universal2
  py311-none-macosx_12_0_arm64
  py311-none-macosx_12_0_universal2
  py311-none-macosx_11_0_arm64
  py311-none-macosx_11_0_universal2
  py311-none-macosx_10_16_universal2
  py311-none-macosx_10_15_universal2
  py311-none-macosx_10_14_universal2
  py311-none-macosx_10_13_universal2
  py311-none-macosx_10_12_universal2
  py311-none-macosx_10_11_universal2
  py311-none-macosx_10_10_universal2
  py311-none-macosx_10_9_universal2
  py311-none-macosx_10_8_universal2
  py311-none-macosx_10_7_universal2
  py311-none-macosx_10_6_universal2
  py311-none-macosx_10_5_universal2
  py311-none-macosx_10_4_universal2
  py310-none-macosx_14_0_arm64
  py310-none-macosx_14_0_universal2
  py310-none-macosx_13_0_arm64
  py310-none-macosx_13_0_universal2
  py310-none-macosx_12_0_arm64
  py310-none-macosx_12_0_universal2
  py310-none-macosx_11_0_arm64
  py310-none-macosx_11_0_universal2
  py310-none-macosx_10_16_universal2
  py310-none-macosx_10_15_universal2
  py310-none-macosx_10_14_universal2
  py310-none-macosx_10_13_universal2
  py310-none-macosx_10_12_universal2
  py310-none-macosx_10_11_universal2
  py310-none-macosx_10_10_universal2
  py310-none-macosx_10_9_universal2
  py310-none-macosx_10_8_universal2
  py310-none-macosx_10_7_universal2
  py310-none-macosx_10_6_universal2
  py310-none-macosx_10_5_universal2
  py310-none-macosx_10_4_universal2
  py39-none-macosx_14_0_arm64
  py39-none-macosx_14_0_universal2
  py39-none-macosx_13_0_arm64
  py39-none-macosx_13_0_universal2
  py39-none-macosx_12_0_arm64
  py39-none-macosx_12_0_universal2
  py39-none-macosx_11_0_arm64
  py39-none-macosx_11_0_universal2
  py39-none-macosx_10_16_universal2
  py39-none-macosx_10_15_universal2
  py39-none-macosx_10_14_universal2
  py39-none-macosx_10_13_universal2
  py39-none-macosx_10_12_universal2
  py39-none-macosx_10_11_universal2
  py39-none-macosx_10_10_universal2
  py39-none-macosx_10_9_universal2
  py39-none-macosx_10_8_universal2
  py39-none-macosx_10_7_universal2
  py39-none-macosx_10_6_universal2
  py39-none-macosx_10_5_universal2
  py39-none-macosx_10_4_universal2
  py38-none-macosx_14_0_arm64
  py38-none-macosx_14_0_universal2
  py38-none-macosx_13_0_arm64
  py38-none-macosx_13_0_universal2
  py38-none-macosx_12_0_arm64
  py38-none-macosx_12_0_universal2
  py38-none-macosx_11_0_arm64
  py38-none-macosx_11_0_universal2
  py38-none-macosx_10_16_universal2
  py38-none-macosx_10_15_universal2
  py38-none-macosx_10_14_universal2
  py38-none-macosx_10_13_universal2
  py38-none-macosx_10_12_universal2
  py38-none-macosx_10_11_universal2
  py38-none-macosx_10_10_universal2
  py38-none-macosx_10_9_universal2
  py38-none-macosx_10_8_universal2
  py38-none-macosx_10_7_universal2
  py38-none-macosx_10_6_universal2
  py38-none-macosx_10_5_universal2
  py38-none-macosx_10_4_universal2
  py37-none-macosx_14_0_arm64
  py37-none-macosx_14_0_universal2
  py37-none-macosx_13_0_arm64
  py37-none-macosx_13_0_universal2
  py37-none-macosx_12_0_arm64
  py37-none-macosx_12_0_universal2
  py37-none-macosx_11_0_arm64
  py37-none-macosx_11_0_universal2
  py37-none-macosx_10_16_universal2
  py37-none-macosx_10_15_universal2
  py37-none-macosx_10_14_universal2
  py37-none-macosx_10_13_universal2
  py37-none-macosx_10_12_universal2
  py37-none-macosx_10_11_universal2
  py37-none-macosx_10_10_universal2
  py37-none-macosx_10_9_universal2
  py37-none-macosx_10_8_universal2
  py37-none-macosx_10_7_universal2
  py37-none-macosx_10_6_universal2
  py37-none-macosx_10_5_universal2
  py37-none-macosx_10_4_universal2
  py36-none-macosx_14_0_arm64
  py36-none-macosx_14_0_universal2
  py36-none-macosx_13_0_arm64
  py36-none-macosx_13_0_universal2
  py36-none-macosx_12_0_arm64
  py36-none-macosx_12_0_universal2
  py36-none-macosx_11_0_arm64
  py36-none-macosx_11_0_universal2
  py36-none-macosx_10_16_universal2
  py36-none-macosx_10_15_universal2
  py36-none-macosx_10_14_universal2
  py36-none-macosx_10_13_universal2
  py36-none-macosx_10_12_universal2
  py36-none-macosx_10_11_universal2
  py36-none-macosx_10_10_universal2
  py36-none-macosx_10_9_universal2
  py36-none-macosx_10_8_universal2
  py36-none-macosx_10_7_universal2
  py36-none-macosx_10_6_universal2
  py36-none-macosx_10_5_universal2
  py36-none-macosx_10_4_universal2
  py35-none-macosx_14_0_arm64
  py35-none-macosx_14_0_universal2
  py35-none-macosx_13_0_arm64
  py35-none-macosx_13_0_universal2
  py35-none-macosx_12_0_arm64
  py35-none-macosx_12_0_universal2
  py35-none-macosx_11_0_arm64
  py35-none-macosx_11_0_universal2
  py35-none-macosx_10_16_universal2
  py35-none-macosx_10_15_universal2
  py35-none-macosx_10_14_universal2
  py35-none-macosx_10_13_universal2
  py35-none-macosx_10_12_universal2
  py35-none-macosx_10_11_universal2
  py35-none-macosx_10_10_universal2
  py35-none-macosx_10_9_universal2
  py35-none-macosx_10_8_universal2
  py35-none-macosx_10_7_universal2
  py35-none-macosx_10_6_universal2
  py35-none-macosx_10_5_universal2
  py35-none-macosx_10_4_universal2
  py34-none-macosx_14_0_arm64
  py34-none-macosx_14_0_universal2
  py34-none-macosx_13_0_arm64
  py34-none-macosx_13_0_universal2
  py34-none-macosx_12_0_arm64
  py34-none-macosx_12_0_universal2
  py34-none-macosx_11_0_arm64
  py34-none-macosx_11_0_universal2
  py34-none-macosx_10_16_universal2
  py34-none-macosx_10_15_universal2
  py34-none-macosx_10_14_universal2
  py34-none-macosx_10_13_universal2
  py34-none-macosx_10_12_universal2
  py34-none-macosx_10_11_universal2
  py34-none-macosx_10_10_universal2
  py34-none-macosx_10_9_universal2
  py34-none-macosx_10_8_universal2
  py34-none-macosx_10_7_universal2
  py34-none-macosx_10_6_universal2
  py34-none-macosx_10_5_universal2
  py34-none-macosx_10_4_universal2
  py33-none-macosx_14_0_arm64
  py33-none-macosx_14_0_universal2
  py33-none-macosx_13_0_arm64
  py33-none-macosx_13_0_universal2
  py33-none-macosx_12_0_arm64
  py33-none-macosx_12_0_universal2
  py33-none-macosx_11_0_arm64
  py33-none-macosx_11_0_universal2
  py33-none-macosx_10_16_universal2
  py33-none-macosx_10_15_universal2
  py33-none-macosx_10_14_universal2
  py33-none-macosx_10_13_universal2
  py33-none-macosx_10_12_universal2
  py33-none-macosx_10_11_universal2
  py33-none-macosx_10_10_universal2
  py33-none-macosx_10_9_universal2
  py33-none-macosx_10_8_universal2
  py33-none-macosx_10_7_universal2
  py33-none-macosx_10_6_universal2
  py33-none-macosx_10_5_universal2
  py33-none-macosx_10_4_universal2
  py32-none-macosx_14_0_arm64
  py32-none-macosx_14_0_universal2
  py32-none-macosx_13_0_arm64
  py32-none-macosx_13_0_universal2
  py32-none-macosx_12_0_arm64
  py32-none-macosx_12_0_universal2
  py32-none-macosx_11_0_arm64
  py32-none-macosx_11_0_universal2
  py32-none-macosx_10_16_universal2
  py32-none-macosx_10_15_universal2
  py32-none-macosx_10_14_universal2
  py32-none-macosx_10_13_universal2
  py32-none-macosx_10_12_universal2
  py32-none-macosx_10_11_universal2
  py32-none-macosx_10_10_universal2
  py32-none-macosx_10_9_universal2
  py32-none-macosx_10_8_universal2
  py32-none-macosx_10_7_universal2
  py32-none-macosx_10_6_universal2
  py32-none-macosx_10_5_universal2
  py32-none-macosx_10_4_universal2
  py31-none-macosx_14_0_arm64
  py31-none-macosx_14_0_universal2
  py31-none-macosx_13_0_arm64
  py31-none-macosx_13_0_universal2
  py31-none-macosx_12_0_arm64
  py31-none-macosx_12_0_universal2
  py31-none-macosx_11_0_arm64
  py31-none-macosx_11_0_universal2
  py31-none-macosx_10_16_universal2
  py31-none-macosx_10_15_universal2
  py31-none-macosx_10_14_universal2
  py31-none-macosx_10_13_universal2
  py31-none-macosx_10_12_universal2
  py31-none-macosx_10_11_universal2
  py31-none-macosx_10_10_universal2
  py31-none-macosx_10_9_universal2
  py31-none-macosx_10_8_universal2
  py31-none-macosx_10_7_universal2
  py31-none-macosx_10_6_universal2
  py31-none-macosx_10_5_universal2
  py31-none-macosx_10_4_universal2
  py30-none-macosx_14_0_arm64
  py30-none-macosx_14_0_universal2
  py30-none-macosx_13_0_arm64
  py30-none-macosx_13_0_universal2
  py30-none-macosx_12_0_arm64
  py30-none-macosx_12_0_universal2
  py30-none-macosx_11_0_arm64
  py30-none-macosx_11_0_universal2
  py30-none-macosx_10_16_universal2
  py30-none-macosx_10_15_universal2
  py30-none-macosx_10_14_universal2
  py30-none-macosx_10_13_universal2
  py30-none-macosx_10_12_universal2
  py30-none-macosx_10_11_universal2
  py30-none-macosx_10_10_universal2
  py30-none-macosx_10_9_universal2
  py30-none-macosx_10_8_universal2
  py30-none-macosx_10_7_universal2
  py30-none-macosx_10_6_universal2
  py30-none-macosx_10_5_universal2
  py30-none-macosx_10_4_universal2
  cp312-none-any
  py312-none-any
  py3-none-any
  py311-none-any
  py310-none-any
  py39-none-any
  py38-none-any
  py37-none-any
  py36-none-any
  py35-none-any
  py34-none-any
  py33-none-any
  py32-none-any
  py31-none-any
  py30-none-any
```
Installation directly with pip succeeds
```
pip install ~/Downloads/pyarrow-17.0.0-cp312-cp312-macosx_11_0_arm64.whl
```

---

_Comment by @charliermarsh on 2024-09-20 03:18_

That's surprising and I'm having trouble explaining it. Are both commands resolving to the same Python interpreter?

---

_Label `needs-mre` added by @charliermarsh on 2024-09-20 03:19_

---

_Comment by @charliermarsh on 2024-09-20 03:19_

(I did just download and successfully install that wheel on my own ARM Mac with Python 3.12.6.)

---

_Comment by @jimmy-marmalade on 2024-09-20 03:41_

Thanks for the response. Different interpreters, but both Python 3.12.6.

---

_Comment by @charliermarsh on 2024-09-20 04:23_

Notice that it's also not accepting any of the pre-built NumPy wheels.

---

_Comment by @charliermarsh on 2024-09-20 04:25_

Actually, can you try running `uv cache clean`, then re-running your command?

---

_Comment by @jimmy-marmalade on 2024-09-20 04:31_

Reproduces failure to fetch pyarrow wheel. Build from source is my issue which is why original post showed attempted installation from prebuilt wheel.

```
➜  monolith git:(main) ✗ uv cache clean
Clearing cache at: /Users/jimmy/.cache/uv
Removed 68250 files (2.1GiB)
➜  monolith git:(main) ✗ uv add pyarrow
Resolved 273 packages in 2.80s
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: pyarrow==17.0.0
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:
running bdist_wheel
running build
running build_py
creating build/lib.macosx-14.1-arm64-cpython-312/benchmarks
copying benchmarks/array_ops.py -> build/lib.macosx-14.1-arm64-cpython-312/benchmarks
copying benchmarks/convert_builtins.py -> build/lib.macosx-14.1-arm64-cpython-312/benchmarks
copying benchmarks/parquet.py -> build/lib.macosx-14.1-arm64-cpython-312/benchmarks
copying benchmarks/io.py -> build/lib.macosx-14.1-arm64-cpython-312/benchmarks
copying benchmarks/__init__.py -> build/lib.macosx-14.1-arm64-cpython-312/benchmarks
copying benchmarks/microbenchmarks.py -> build/lib.macosx-14.1-arm64-cpython-312/benchmarks
copying benchmarks/convert_pandas.py -> build/lib.macosx-14.1-arm64-cpython-312/benchmarks
copying benchmarks/common.py -> build/lib.macosx-14.1-arm64-cpython-312/benchmarks
copying benchmarks/streaming.py -> build/lib.macosx-14.1-arm64-cpython-312/benchmarks
creating build/lib.macosx-14.1-arm64-cpython-312/scripts
copying scripts/test_imports.py -> build/lib.macosx-14.1-arm64-cpython-312/scripts
copying scripts/test_leak.py -> build/lib.macosx-14.1-arm64-cpython-312/scripts
copying scripts/run_emscripten_tests.py -> build/lib.macosx-14.1-arm64-cpython-312/scripts
creating build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/orc.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/conftest.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_generated_version.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/benchmark.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_compute_docstrings.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/ipc.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/util.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/flight.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/cffi.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/substrait.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/__init__.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/types.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/dataset.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/cuda.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/feather.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/pandas_compat.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/fs.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/acero.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/csv.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/jvm.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/json.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/compute.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
creating build/lib.macosx-14.1-arm64-cpython-312/examples/parquet_encryption
copying examples/parquet_encryption/sample_vault_kms_client.py -> build/lib.macosx-14.1-arm64-cpython-312/examples/parquet_encryption
creating build/lib.macosx-14.1-arm64-cpython-312/examples/dataset
copying examples/dataset/write_dataset_encrypted.py -> build/lib.macosx-14.1-arm64-cpython-312/examples/dataset
creating build/lib.macosx-14.1-arm64-cpython-312/examples/flight
copying examples/flight/server.py -> build/lib.macosx-14.1-arm64-cpython-312/examples/flight
copying examples/flight/client.py -> build/lib.macosx-14.1-arm64-cpython-312/examples/flight
copying examples/flight/middleware.py -> build/lib.macosx-14.1-arm64-cpython-312/examples/flight
creating build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_tensor.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_ipc.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_flight_async.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/conftest.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_convert_builtin.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_misc.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/arrow_39313.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/arrow_16597.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_acero.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_gandiva.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/strategies.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_adhoc_memory_leak.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/arrow_7980.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/util.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_orc.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_table.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_array.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_deprecations.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/__init__.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_io.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_util.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_cuda_numba_interop.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_cpp_internals.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_cffi.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_schema.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_jvm.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_fs.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_udf.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/pandas_threaded_import.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/pandas_examples.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_cython.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_sparse_tensor.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_dataset.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_builder.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_dataset_encryption.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_cuda.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_extension_type.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_feather.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_pandas.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_memory.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_exec_plan.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_flight.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_device.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/read_record_batch.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_dlpack.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_json.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_compute.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_strategies.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_csv.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_scalars.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_gdb.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_types.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/test_substrait.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
creating build/lib.macosx-14.1-arm64-cpython-312/pyarrow/interchange
copying pyarrow/interchange/from_dataframe.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/interchange
copying pyarrow/interchange/dataframe.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/interchange
copying pyarrow/interchange/__init__.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/interchange
copying pyarrow/interchange/buffer.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/interchange
copying pyarrow/interchange/column.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/interchange
creating build/lib.macosx-14.1-arm64-cpython-312/pyarrow/vendored
copying pyarrow/vendored/version.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/vendored
copying pyarrow/vendored/__init__.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/vendored
copying pyarrow/vendored/docscrape.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/vendored
creating build/lib.macosx-14.1-arm64-cpython-312/pyarrow/parquet
copying pyarrow/parquet/encryption.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/parquet
copying pyarrow/parquet/__init__.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/parquet
copying pyarrow/parquet/core.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/parquet
creating build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/interchange
copying pyarrow/tests/interchange/__init__.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/interchange
copying pyarrow/tests/interchange/test_conversion.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/interchange
copying pyarrow/tests/interchange/test_interchange_spec.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/interchange
creating build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/parquet
copying pyarrow/tests/parquet/test_basic.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/parquet
copying pyarrow/tests/parquet/conftest.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/parquet
copying pyarrow/tests/parquet/encryption.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/parquet
copying pyarrow/tests/parquet/test_parquet_writer.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/parquet
copying pyarrow/tests/parquet/test_metadata.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/parquet
copying pyarrow/tests/parquet/__init__.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/parquet
copying pyarrow/tests/parquet/test_datetime.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/parquet
copying pyarrow/tests/parquet/common.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/parquet
copying pyarrow/tests/parquet/test_dataset.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/parquet
copying pyarrow/tests/parquet/test_data_types.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/parquet
copying pyarrow/tests/parquet/test_pandas.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/parquet
copying pyarrow/tests/parquet/test_parquet_file.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/parquet
copying pyarrow/tests/parquet/test_compliant_nested_type.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/parquet
copying pyarrow/tests/parquet/test_encryption.py -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/parquet
running egg_info
writing pyarrow.egg-info/PKG-INFO
writing dependency_links to pyarrow.egg-info/dependency_links.txt
writing requirements to pyarrow.egg-info/requires.txt
writing top-level names to pyarrow.egg-info/top_level.txt
reading manifest file 'pyarrow.egg-info/SOURCES.txt'
reading manifest template 'MANIFEST.in'
writing manifest file 'pyarrow.egg-info/SOURCES.txt'
creating build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/AWSSDKVariables.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/BuildUtils.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/DefineOptions.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindAWSSDKAlt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindAzure.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindBrotliAlt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindClangTools.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindGTestAlt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindInferTools.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindLLVMAlt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindOpenSSLAlt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindProtobufAlt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindPython3Alt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindRapidJSONAlt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindSQLite3Alt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindSnappyAlt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindThriftAlt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/Findc-aresAlt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindgRPCAlt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindgflagsAlt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindglogAlt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindjemallocAlt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/Findlibrados.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/Findlz4Alt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindorcAlt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/Findre2Alt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/Findutf8proc.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/FindzstdAlt.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/GandivaAddBitcode.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/SetupCxxFlags.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/ThirdpartyToolchain.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/UseCython.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/Usevcpkg.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/aws_sdk_cpp_generate_variables.sh -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/san-config.cmake -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying cmake_modules/snappy.diff -> build/lib.macosx-14.1-arm64-cpython-312/cmake_modules
copying pyarrow/__init__.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_acero.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_acero.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_azurefs.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_compute.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_compute.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_csv.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_csv.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_cuda.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_cuda.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_dataset.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_dataset.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_dataset_orc.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_dataset_parquet.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_dataset_parquet.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_dataset_parquet_encryption.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_dlpack.pxi -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_feather.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_flight.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_fs.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_fs.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_gcsfs.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_hdfs.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_json.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_json.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_orc.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_orc.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_parquet.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_parquet.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_parquet_encryption.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_parquet_encryption.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_pyarrow_cpp_tests.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_pyarrow_cpp_tests.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_s3fs.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/_substrait.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/array.pxi -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/benchmark.pxi -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/builder.pxi -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/compat.pxi -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/config.pxi -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/device.pxi -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/error.pxi -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/gandiva.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/io.pxi -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/ipc.pxi -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/lib.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/lib.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/memory.pxi -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/pandas-shim.pxi -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/public-api.pxi -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/scalar.pxi -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/table.pxi -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/tensor.pxi -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
copying pyarrow/types.pxi -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow
creating build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/common.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow_feather.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow_acero.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libparquet_encryption.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/__init__.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libgandiva.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow_python.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow_flight.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow_dataset_parquet.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow_dataset.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow_substrait.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow_cuda.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow_fs.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/tests/bound_function_visit_strings.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/extensions.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/tests/pyarrow_cython_example.pyx -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests
copying pyarrow/includes/__init__.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/common.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow_acero.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow_cuda.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow_dataset.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow_dataset_parquet.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow_feather.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow_flight.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow_fs.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow_python.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libarrow_substrait.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libgandiva.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
copying pyarrow/includes/libparquet_encryption.pxd -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/includes
creating build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/data/feather
copying pyarrow/tests/data/feather/v0.17.0.version.2-compression.lz4.feather -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/data/feather
creating build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/data/orc
copying pyarrow/tests/data/orc/README.md -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/data/orc
copying pyarrow/tests/data/orc/TestOrcFile.emptyFile.jsn.gz -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/data/orc
copying pyarrow/tests/data/orc/TestOrcFile.emptyFile.orc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/data/orc
copying pyarrow/tests/data/orc/TestOrcFile.test1.jsn.gz -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/data/orc
copying pyarrow/tests/data/orc/TestOrcFile.test1.orc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/data/orc
copying pyarrow/tests/data/orc/TestOrcFile.testDate1900.jsn.gz -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/data/orc
copying pyarrow/tests/data/orc/TestOrcFile.testDate1900.orc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/data/orc
copying pyarrow/tests/data/orc/decimal.jsn.gz -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/data/orc
copying pyarrow/tests/data/orc/decimal.orc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/data/orc
creating build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/data/parquet
copying pyarrow/tests/data/parquet/v0.7.1.all-named-index.parquet -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/data/parquet
copying pyarrow/tests/data/parquet/v0.7.1.column-metadata-handling.parquet -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/data/parquet
copying pyarrow/tests/data/parquet/v0.7.1.parquet -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/data/parquet
copying pyarrow/tests/data/parquet/v0.7.1.some-named-index.parquet -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/tests/data/parquet
creating build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/CMakeLists.txt -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/api.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/arrow_to_pandas.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/arrow_to_pandas.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/arrow_to_python_internal.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/async.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/benchmark.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/benchmark.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/common.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/common.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/csv.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/csv.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/datetime.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/datetime.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/decimal.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/decimal.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/deserialize.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/deserialize.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/extension_type.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/extension_type.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/filesystem.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/filesystem.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/flight.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/flight.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/gdb.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/gdb.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/helpers.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/helpers.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/inference.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/inference.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/init.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/init.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/io.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/io.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/ipc.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/ipc.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/iterators.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/numpy_convert.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/numpy_convert.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/numpy_internal.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/numpy_interop.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/numpy_to_arrow.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/numpy_to_arrow.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/parquet_encryption.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/parquet_encryption.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/pch.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/platform.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/pyarrow.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/pyarrow.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/pyarrow_api.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/pyarrow_lib.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/python_test.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/python_test.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/python_to_arrow.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/python_to_arrow.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/serialize.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/serialize.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/type_traits.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/udf.cc -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/udf.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
copying pyarrow/src/arrow/python/visibility.h -> build/lib.macosx-14.1-arm64-cpython-312/pyarrow/src/arrow/python
running build_ext
creating /Users/jimmy/.cache/uv/built-wheels-v3/pypi/pyarrow/17.0.0/nF9fTeHmvSk2k9OouXuQL/pyarrow-17.0.0.tar.gz/build/temp.macosx-14.1-arm64-cpython-312
-- Running cmake for PyArrow
cmake -DCMAKE_INSTALL_PREFIX=/Users/jimmy/.cache/uv/built-wheels-v3/pypi/pyarrow/17.0.0/nF9fTeHmvSk2k9OouXuQL/pyarrow-17.0.0.tar.gz/build/lib.macosx-14.1-arm64-cpython-312/pyarrow -DPYTHON_EXECUTABLE=/Users/jimmy/.cache/uv/builds-v0/.tmp0SACCW/bin/python -DPython3_EXECUTABLE=/Users/jimmy/.cache/uv/builds-v0/.tmp0SACCW/bin/python -DPYARROW_CXXFLAGS= -DPYARROW_BUNDLE_ARROW_CPP=off -DPYARROW_BUNDLE_CYTHON_CPP=off -DPYARROW_GENERATE_COVERAGE=off -DCMAKE_BUILD_TYPE=release /Users/jimmy/.cache/uv/built-wheels-v3/pypi/pyarrow/17.0.0/nF9fTeHmvSk2k9OouXuQL/pyarrow-17.0.0.tar.gz
--- stderr:
<string>:34: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
ERROR setuptools_scm._file_finders.git listing git files failed - pretending there aren't any
warning: no files found matching '../LICENSE.txt'
warning: no files found matching '../NOTICE.txt'
warning: no previously-included files matching '*.so' found anywhere in distribution
warning: no previously-included files matching '*.pyc' found anywhere in distribution
warning: no previously-included files matching '*~' found anywhere in distribution
warning: no previously-included files matching '#*' found anywhere in distribution
warning: no previously-included files matching '.git*' found anywhere in distribution
warning: no previously-included files matching '.DS_Store' found anywhere in distribution
no previously-included directories found matching '.asv'
error: command 'cmake' failed: No such file or directory
---
```

---

_Comment by @emsi on 2024-09-20 17:53_

```
error: command 'cmake' failed: No such file or directory
```

`apt install cmake` should help.

However even with cmake the build fails:

```
running build_ext
creating /home/emsi/.cache/uv/sdists-v4/pypi/pyarrow/13.0.0/2cHRlmtubQu_itP-S78ZP/pyarrow-13.0.0.tar.gz/build/temp.linux-x86_64-cpython-312
-- Running cmake for PyArrow
cmake -DCMAKE_INSTALL_PREFIX=/home/emsi/.cache/uv/sdists-v4/pypi/pyarrow/13.0.0/2cHRlmtubQu_itP-S78ZP/pyarrow-13.0.0.tar.gz/build/lib.linux-x86_64-cpython-312/pyarrow -DPYTHON_EXECUTABLE=/home/emsi/.cache/uv/builds-v0/.tmpuspTfZ/bin/python -DPython3_EXECUTABLE=/home/emsi/.cache/uv/builds-v0/.tmpuspTfZ/bin/python -DPYARROW_CXXFLAGS= -DPYARROW_BUILD_CUDA=off -DPYARROW_BUILD_SUBSTRAIT=off -DPYARROW_BUILD_FLIGHT=off -DPYARROW_BUILD_GANDIVA=off -DPYARROW_BUILD_ACERO=off -DPYARROW_BUILD_DATASET=off -DPYARROW_BUILD_ORC=off -DPYARROW_BUILD_PARQUET=off -DPYARROW_BUILD_PARQUET_ENCRYPTION=off -DPYARROW_BUILD_GCS=off -DPYARROW_BUILD_S3=off -DPYARROW_BUILD_HDFS=off -DPYARROW_BUNDLE_ARROW_CPP=off -DPYARROW_BUNDLE_CYTHON_CPP=off -DPYARROW_GENERATE_COVERAGE=off -DCMAKE_BUILD_TYPE=release /home/emsi/.cache/uv/sdists-v4/pypi/pyarrow/13.0.0/2cHRlmtubQu_itP-S78ZP/pyarrow-13.0.0.tar.gz
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- System processor: x86_64
-- Performing Test CXX_SUPPORTS_SSE4_2
-- Performing Test CXX_SUPPORTS_SSE4_2 - Success
-- Performing Test CXX_SUPPORTS_AVX2
-- Performing Test CXX_SUPPORTS_AVX2 - Success
-- Performing Test CXX_SUPPORTS_AVX512
-- Performing Test CXX_SUPPORTS_AVX512 - Success
-- Arrow build warning level: PRODUCTION
-- Using ld linker
-- Build Type: RELEASE
-- CMAKE_C_FLAGS:  -Wall -fno-semantic-interposition -msse4.2  -fdiagnostics-color=always  -fno-omit-frame-pointer -Wno-unused-variable -Wno-maybe-uninitialized
-- CMAKE_CXX_FLAGS:  -Wno-noexcept-type  -Wall -fno-semantic-interposition -msse4.2  -fdiagnostics-color=always  -fno-omit-frame-pointer -Wno-unused-variable -Wno-maybe-uninitialized
-- Generator: Unix Makefiles
-- Build output directory: /home/emsi/.cache/uv/sdists-v4/pypi/pyarrow/13.0.0/2cHRlmtubQu_itP-S78ZP/pyarrow-13.0.0.tar.gz/build/temp.linux-x86_64-cpython-312/release
-- Found Python3: /home/emsi/.cache/uv/builds-v0/.tmpuspTfZ/bin/python (found version "3.12.6") found components: Interpreter Development.Module NumPy 
-- Found Python3Alt: /home/emsi/.cache/uv/builds-v0/.tmpuspTfZ/bin/python  
-- Configuring incomplete, errors occurred!
See also "/home/emsi/.cache/uv/sdists-v4/pypi/pyarrow/13.0.0/2cHRlmtubQu_itP-S78ZP/pyarrow-13.0.0.tar.gz/build/temp.linux-x86_64-cpython-312/CMakeFiles/CMakeOutput.log".
--- stderr:
<string>:34: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
/home/emsi/.cache/uv/builds-v0/.tmpuspTfZ/lib/python3.12/site-packages/setuptools/_distutils/dist.py:261: UserWarning: Unknown distribution option: 'tests_require'
  warnings.warn(msg)
/home/emsi/.cache/uv/builds-v0/.tmpuspTfZ/lib/python3.12/site-packages/setuptools/_distutils/dist.py:261: UserWarning: Unknown distribution option: 'test_suite'
  warnings.warn(msg)
WARNING setuptools_scm.pyproject_reading toml section missing 'pyproject.toml does not contain a tool.setuptools_scm section'
Traceback (most recent call last):
  File "/home/emsi/.cache/uv/builds-v0/.tmpuspTfZ/lib/python3.12/site-packages/setuptools_scm/_integration/pyproject_reading.py", line 36, in read_pyproject
    section = defn.get("tool", {})[tool_name]
              ~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^
KeyError: 'setuptools_scm'
ERROR setuptools_scm._file_finders.git listing git files failed - pretending there aren't any
warning: no files found matching '../LICENSE.txt'
warning: no files found matching '../NOTICE.txt'
warning: no previously-included files matching '*.so' found anywhere in distribution
warning: no previously-included files matching '*.pyc' found anywhere in distribution
warning: no previously-included files matching '*~' found anywhere in distribution
warning: no previously-included files matching '#*' found anywhere in distribution
warning: no previously-included files matching '.git*' found anywhere in distribution
warning: no previously-included files matching '.DS_Store' found anywhere in distribution
no previously-included directories found matching '.asv'
CMake Error at CMakeLists.txt:261 (find_package):
  By not providing "FindArrow.cmake" in CMAKE_MODULE_PATH this project has
  asked CMake to find a package configuration file provided by "Arrow", but
  CMake did not find one.

  Could not find a package configuration file provided by "Arrow" with any of
  the following names:

    ArrowConfig.cmake
    arrow-config.cmake

  Add the installation prefix of "Arrow" to CMAKE_PREFIX_PATH or set
  "Arrow_DIR" to a directory containing one of the above files.  If "Arrow"
  provides a separate development package or SDK, be sure it has been
  installed.


error: command '/usr/bin/cmake' failed with exit code 1

```

---

_Comment by @pascalwhoop on 2024-10-15 15:08_

I had this issue only on python 3.13, using `uv venv -p 3.10` fixed it for me

---

_Comment by @gcpp003 on 2024-10-16 04:48_

I had the same error with python version 3.13.0. With python version 3.12.7 it worked normally.

```
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: pyarrow==17.0.0
  Caused by: Build backend failed to build wheel through `build_wheel` (exit status: 1)
```

`error: command 'cmake' failed: No such file or directory`

---

_Comment by @charliermarsh on 2024-10-16 12:42_

@gcpp003 -- I suspect that's because `pyarrow` does not publish Python 3.13 wheels (you can see that there are no `cp313` references [here](https://pypi.org/project/pyarrow/17.0.0/#files)). So when you install `pyarrow` on Python 3.13, you have to build from source, unlike when you install on Python 3.12.

---

_Comment by @jimmy-marmalade on 2024-11-05 20:24_

@charliermarsh is there any other info I can provide to help here?

---

_Comment by @ChristophrK on 2025-03-07 18:59_

> [@gcpp003](https://github.com/gcpp003) -- I suspect that's because `pyarrow` does not publish Python 3.13 wheels (you can see that there are no `cp313` references [here](https://pypi.org/project/pyarrow/17.0.0/#files)). So when you install `pyarrow` on Python 3.13, you have to build from source, unlike when you install on Python 3.12.

I ran into this issue when updating my project from Python 3.10 to Python 3.13. But after running `uv cache clean` `pyarrow` installs without issue, since for `pyarrow>=18.0.0` Python 3.13 wheels are published (see https://pypi.org/project/pyarrow/18.0.0/#files). So for the recent `pyarrow` versions this issue is fixed and from my perspective this issue can be closed.

---

_Comment by @zanieb on 2025-03-07 19:03_

Thank you!

---

_Closed by @zanieb on 2025-03-07 19:03_

---

_Comment by @jimmy-marmalade on 2025-03-14 01:36_

@zanieb, OP here and wanted to reopen because I'm still running into the same issue. Happy to provide any debugging info that may be helpful.

---

_Comment by @jimmy-marmalade on 2025-03-14 01:45_

Also to @charliermarsh 's question about using the same python interpreter and demonstrating that installation via pip works, the following works:

```
cd /tmp
mkdir uv-test
cd uv-test
uv init
uv add pip
./.venv/bin/pip install pyarrow
``` 

---

_Comment by @MentalMegalodon on 2025-03-27 17:43_

I'm having the same issue where only `uv` is failing to install pyarrow's older versions, and I can't update pyarrow because a library I depend on has the older version pinned.

---

_Comment by @shipmints on 2025-03-31 18:08_

> I'm having the same issue where only `uv` is failing to install pyarrow's older versions, and I can't update pyarrow because a library I depend on has the older version pinned.

If you're on macOS, `SYSTEM_VERSION_COMPAT=0 uv xxx` and see if that helps wheel resolution.

---

_Comment by @d0plr on 2025-09-19 12:18_

Hi,
I'm on macOS x86_64, Python 3.13, and I have the same issue than MentalMegoldon: `uv` is failing to compile pyarrow, and I cannot update it because another dependency needs a specific version (`pyarrow==10.0.1`).

The error log ends with:
```
      CMake Error at CMakeLists.txt:63 (find_package):
        By not providing "FindArrow.cmake" in CMAKE_MODULE_PATH this project has
        asked CMake to find a package configuration file provided by "Arrow", but
        CMake did not find one.

        Could not find a package configuration file provided by "Arrow" with any of
        the following names:

          ArrowConfig.cmake
          arrow-config.cmake

        Add the installation prefix of "Arrow" to CMAKE_PREFIX_PATH or set
        "Arrow_DIR" to a directory containing one of the above files.  If "Arrow"
        provides a separate development package or SDK, be sure it has been
        installed.


      error: command '/opt/local/bin/cmake' failed with exit code 1
```

Using `SYSTEM_VERSION_COMPAT=0 uv xxx` did not help.


---

_Comment by @zanieb on 2025-09-22 04:21_

@d0plr you should report build errors with pyarrow to the pyarrow team — we can't usually fix those. Please open a new issue if you're still having problems .I'd encourage you to ask your dependency that requires the older pyarrow version to upgrade, or you can use the `overrides-dependencies` to relax their constraint.

My understanding here is that you can't use old pyarrow versions on Python 3.13, so if you need to use an older pyarrow version you also need to use an older Python version.

---
