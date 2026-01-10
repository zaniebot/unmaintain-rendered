```yaml
number: 11038
title: "Failure to `uv pip install vllm==0.7.0` on macOS"
type: issue
state: closed
author: jamesbraza
labels:
  - bug
assignees: []
created_at: 2025-01-28T20:18:11Z
updated_at: 2025-02-11T22:26:41Z
url: https://github.com/astral-sh/uv/issues/11038
synced_at: 2026-01-10T03:50:31Z
```

# Failure to `uv pip install vllm==0.7.0` on macOS

---

_Issue opened by @jamesbraza on 2025-01-28 20:18_

### Summary

Running this command fails:

```bash
uv cache clean
uv pip install vllm==0.7.0
```

With this message:

```none
Resolved 113 packages in 1.28s
  × Failed to download and build `vllm==0.7.0`
  ╰─▶ Package metadata version `0.7.0+cpu` does not match given version `0.7.0`
```

Any chance we can get `+cpu` autoresolving to work for latest `vllm`?

### Platform

Darwin 24.1.0 arm64

### Version

uv 0.5.24 (42fae925c 2025-01-23)

### Python version

3.12.8

---

_Label `bug` added by @jamesbraza on 2025-01-28 20:18_

---

_Comment by @charliermarsh on 2025-01-28 20:21_

This is a real bug in the package. The wheel filename and the metadata contained inside the wheel are not consistent.

---

_Comment by @jamesbraza on 2025-01-28 20:31_

Okay, feel free to close this out then if there's nothing `uv` can do, I can open an issue in `vLLM`.

---

_Comment by @charliermarsh on 2025-01-28 20:32_

Oh, this is the most recent version? Hmm, bummer.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-28 20:34_

---

_Comment by @charliermarsh on 2025-01-28 20:48_

We could ignore the discrepancy, I just don't know what will break. There will definitely be some wonky behavior though.

---

_Comment by @Qubitium on 2025-02-10 08:29_

@charliermarsh At [GPTQModel](https://github.com/ModelCloud/GPTQModel/issues/1238) We are trying to fix this uv compat issue where a pre-compiled wheel (dowloaded) would not work as uv complains correctly or incorrectly about name mistmatch.

Can you tell let us exactly how `uv` is using metadata in setup.py to do name mismatch checking? Or basically what internal pkg meta field `uv` uses from wheel and setup.py to reach this mismatch error. Thanks!

```
uv pip install -v gptqmodel --no-build-isolation
```

```
Building gptqmodel==1.8.1
DEBUG Building: gptqmodel==1.8.1
DEBUG Proceeding without build isolation
DEBUG Calling setuptools.build_meta:__legacy__.build_wheel("/nfs/scistore19/alistgrp/dkuznede/.cache/uv/builds-v0/.tmpmWmvcU", {}, None)
DEBUG conda_cuda_include_dir /nfs/scistore19/alistgrp/dkuznede/openr1/lib/python3.11/site-packages/nvidia/cuda_runtime/include
DEBUG appending conda cuda include dir /nfs/scistore19/alistgrp/dkuznede/openr1/lib/python3.11/site-packages/nvidia/cuda_runtime/include
DEBUG running bdist_wheel
DEBUG Guessing wheel URL: https://github.com/ModelCloud/GPTQModel/releases/download/v1.8.1/gptqmodel-1.8.1+cu124torch2.5-cp311-cp311-linux_x86_64.whl
DEBUG wheel name=gptqmodel-1.8.1+cu124torch2.5-cp311-cp311-linux_x86_64.whl
DEBUG Raw wheel path /nfs/scistore19/alistgrp/dkuznede/.cache/uv/builds-v0/.tmpmWmvcU/.tmp-nluo6lz5/gptqmodel-1.8.1+cu124torch2.5-cp311-cp311-linux_x86_64.whl
DEBUG Released lock at /nfs/scistore19/alistgrp/dkuznede/.cache/uv/sdists-v7/pypi/gptqmodel/1.8.1/.lock
  × Failed to download and build gptqmodel==1.8.1
  ╰─▶ Package metadata version 1.8.1+cu124torch2.5 does not match given version 1.8.1
```

---

_Comment by @charliermarsh on 2025-02-10 18:31_

@Qubitium -- I believe this is comparing the version reported by the registry (the "given version") against the version in the `.dist-info/METADATA` file within the built wheel.

---

_Comment by @Qubitium on 2025-02-11 04:03_

@charliermarsh Yes. But who is wrong here. I am confused. 

Logic:

1. in `setup.py` we generate a version based on env, cuda, torch etc, and check github reledases to see if there a hosted, pre-compiled, wheel. 
2. if yes, download and install the wheel instead of gating nvcc compilation which make take 10 minutes +

UV complains the that following `metadata` doesn't match for https://github.com/ModelCloud/GPTQModel/releases/download/v1.8.1/gptqmodel-1.8.1+cu121torch2.5-cp310-cp310-linux_x86_64.whl
 

```
Metadata-Version: 2.2
Name: gptqmodel
Version: 1.8.1+cu121torch2.5
``` 

```
 × Failed to download and build gptqmodel==1.8.1
  ╰─▶ Package metadata version 1.8.1+cu124torch2.5 does not match given version 1.8.1
```

UV is just comparing `1.8.1` to `1.8.1+cu121torch2.5` which returns `False` but `1.8.1+cu121torch2.5` is valid according to native pip setuptools?

The bug only happens for wheel that is jit/auto-downloaded from the network via `setup.py`. If you use uv to installt he wheel fie directly it has no error since it doesn't do version check.



---

_Comment by @charliermarsh on 2025-02-11 13:05_

I think it’s comparing the expected version of the source distribution (from the file name, or from the registry) against the version of the built wheel. Can you share more on your setup? How is the dependency configured? Are you installing via direct URL? Or is this package hosted on a registry?

---

_Comment by @CSY-ModelCloud on 2025-02-11 14:40_

@charliermarsh 

https://github.com/ModelCloud/GPTQModel/blob/fd2bbec204ac7e5e5ea1070c4adc4575da98d4f8/setup.py#L283


Iin setup.py, we have a CachedWheelsCommand class. It will try to get current version's prebuit whl from github release.

If downloaded, it will return this wheel file to wheel api, so user won't need to wait 10 minutes to build.

here's the case:

`uv pip install gptqmodel`  actually will try to install latest version, it's `uv pip install gptqmodel==1.8.1`

but in a whl's meta, its version is version+tag, like 1.8.1+cu121torch2.5

so, when uv compares version 1.8.1+cu121torch2.5 is not 1.8.1, it returns false

but pip itself won't treat it as a false

we wan't to know why uv & pip has different result or what to do to fix for uv like add api in setup.py

---

_Comment by @charliermarsh on 2025-02-11 15:57_

Yeah, the issue is that the the `setup.py` reports that the version is `gptqmodel_version`. But when you "build" it, it then returns a wheel with a different version (e.g., `1.8.1+cu121torch2.5`). Those aren't the same version, so we throw an error. Imagine if `gptqmodel_version` were, like `1.0`, and then you built an returned a wheel with version `2.0`. It would be correct to fail in that case. It's the same thing here: `1.8.1` and `1.8.1+cu121torch2.5` are not the same version. We could change it such that we allow this incompatibility, but it would need to be an intentional change on our end. (I don't know if pip's lack of error is intentional, I'd have to check.)


---

_Comment by @charliermarsh on 2025-02-11 16:01_

I will review pip's behavior and determine whether we should make a change here.

---

_Closed by @charliermarsh on 2025-02-11 22:26_

---

_Closed by @charliermarsh on 2025-02-11 22:26_

---
