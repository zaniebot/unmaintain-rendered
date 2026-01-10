---
number: 11582
title: "UV can't resolve an environment with pyspark"
type: issue
state: closed
author: oresttokovenko
labels:
  - bug
assignees: []
created_at: 2025-02-17T20:17:56Z
updated_at: 2025-02-17T20:23:59Z
url: https://github.com/astral-sh/uv/issues/11582
synced_at: 2026-01-10T01:25:07Z
---

# UV can't resolve an environment with pyspark

---

_Issue opened by @oresttokovenko on 2025-02-17 20:17_

### Summary

Basically I just can't install pyspark, uv hangs when I run `uv add pyspark`.

```bash
dir on main [?] is 📦 v0.1.0 via 🐍 v3.11.11 (python-glue) 
✦ ❯ uv add pyarrow       
⠧ pyspark==3.5.2  
```

It adds the library to the pyproject.toml but it's not able to resolve it. 
```toml
[project]
name = "python-glue"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11, <3.12"
dependencies = [
    "boto3>=1.36.21",
    "pyarrow>=19.0.0",
    "pyspark==3.5.2",
]
```

Here is the uv.lock file
```
version = 1
requires-python = "==3.11.*"

[[package]]
name = "boto3"
version = "1.36.21"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "botocore" },
    { name = "jmespath" },
    { name = "s3transfer" },
]
sdist = { url = "https://files.pythonhosted.org/packages/af/cb/745ca9a661be42f3dc0c5b6ea4d3182d9dd5dfd4204aad4910af20775a26/boto3-1.36.21.tar.gz", hash = "sha256:41eb2b73eb612d300e629e3328b83f1ffea0fc6633e75c241a72a76746c1db26", size = 110999 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/39/99/7f5c7a16e205e19089e7f0d8716e9d1a5207bf4736f82a7d0c602bd0a40c/boto3-1.36.21-py3-none-any.whl", hash = "sha256:f94faa7cf932d781f474d87f8b4c14a033af95ac1460136b40d75e7a30086ef0", size = 139179 },
]

[[package]]
name = "botocore"
version = "1.36.21"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "jmespath" },
    { name = "python-dateutil" },
    { name = "urllib3" },
]
sdist = { url = "https://files.pythonhosted.org/packages/69/9f/17b7610f2bfc5ccba6d2395f1cc856dd3e7e50f0088fc22949e56ae9f569/botocore-1.36.21.tar.gz", hash = "sha256:da746240e2ad64fd4997f7f3664a0a8e303d18075fc1d473727cb6375080ea16", size = 13523380 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/7b/b4/8f1dc71437d12a61ca1daac534bc32fa6ccf207011eab7465d8c8a46dc06/botocore-1.36.21-py3-none-any.whl", hash = "sha256:24a7052e792639dc2726001bd474cd0aaa959c1e18ddd92c17f3adc6efa1b132", size = 13352864 },
]

[[package]]
name = "jmespath"
version = "1.0.1"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/00/2a/e867e8531cf3e36b41201936b7fa7ba7b5702dbef42922193f05c8976cd6/jmespath-1.0.1.tar.gz", hash = "sha256:90261b206d6defd58fdd5e85f478bf633a2901798906be2ad389150c5c60edbe", size = 25843 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/31/b4/b9b800c45527aadd64d5b442f9b932b00648617eb5d63d2c7a6587b7cafc/jmespath-1.0.1-py3-none-any.whl", hash = "sha256:02e2e4cc71b5bcab88332eebf907519190dd9e6e82107fa7f83b1003a6252980", size = 20256 },
]

[[package]]
name = "pyarrow"
version = "19.0.0"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/7b/01/fe1fd04744c2aa038e5a11c7a4adb3d62bce09798695e54f7274b5977134/pyarrow-19.0.0.tar.gz", hash = "sha256:8d47c691765cf497aaeed4954d226568563f1b3b74ff61139f2d77876717084b", size = 1129096 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/82/42/fba3a35bef5833bf88ed35e6a810dc1781236e1d4f808d2df824a7d21819/pyarrow-19.0.0-cp311-cp311-macosx_12_0_arm64.whl", hash = "sha256:8e3a839bf36ec03b4315dc924d36dcde5444a50066f1c10f8290293c0427b46a", size = 30711936 },
    { url = "https://files.pythonhosted.org/packages/88/7a/0da93a3eaaf251a30e32f3221e874263cdcd366c2cd6b7c05293aad91152/pyarrow-19.0.0-cp311-cp311-macosx_12_0_x86_64.whl", hash = "sha256:ce42275097512d9e4e4a39aade58ef2b3798a93aa3026566b7892177c266f735", size = 32133182 },
    { url = "https://files.pythonhosted.org/packages/2f/df/fe43b1c50d3100d0de53f988344118bc20362d0de005f8a407454fa565f8/pyarrow-19.0.0-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:9348a0137568c45601b031a8d118275069435f151cbb77e6a08a27e8125f59d4", size = 41145489 },
    { url = "https://files.pythonhosted.org/packages/45/bb/6f73b41b342a0342f2516a02db4aa97a4f9569cc35482a5c288090140cd4/pyarrow-19.0.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:2a0144a712d990d60f7f42b7a31f0acaccf4c1e43e957f7b1ad58150d6f639c1", size = 42177823 },
    { url = "https://files.pythonhosted.org/packages/23/7b/f038a96f421e453a71bd7a0f78d62b1b2ae9bcac06ed51179ca532e6a0a2/pyarrow-19.0.0-cp311-cp311-manylinux_2_28_aarch64.whl", hash = "sha256:2a1a109dfda558eb011e5f6385837daffd920d54ca00669f7a11132d0b1e6042", size = 40530609 },
    { url = "https://files.pythonhosted.org/packages/b8/39/a2a6714b471c000e6dd6af4495dce00d7d1332351b8e3170dfb9f91dad1f/pyarrow-19.0.0-cp311-cp311-manylinux_2_28_x86_64.whl", hash = "sha256:be686bf625aa7b9bada18defb3a3ea3981c1099697239788ff111d87f04cd263", size = 42081534 },
    { url = "https://files.pythonhosted.org/packages/6c/a3/8396fb06ca05d807e89980c177be26617aad15211ece3184e0caa730b8a6/pyarrow-19.0.0-cp311-cp311-win_amd64.whl", hash = "sha256:239ca66d9a05844bdf5af128861af525e14df3c9591bcc05bac25918e650d3a2", size = 25281090 },
]

[[package]]
name = "python-dateutil"
version = "2.9.0.post0"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "six" },
]
sdist = { url = "https://files.pythonhosted.org/packages/66/c0/0c8b6ad9f17a802ee498c46e004a0eb49bc148f2fd230864601a86dcf6db/python-dateutil-2.9.0.post0.tar.gz", hash = "sha256:37dd54208da7e1cd875388217d5e00ebd4179249f90fb72437e91a35459a0ad3", size = 342432 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/ec/57/56b9bcc3c9c6a792fcbaf139543cee77261f3651ca9da0c93f5c1221264b/python_dateutil-2.9.0.post0-py2.py3-none-any.whl", hash = "sha256:a8b2bc7bffae282281c8140a97d3aa9c14da0b136dfe83f850eea9a5f7470427", size = 229892 },
]

[[package]]
name = "python-glue"
version = "0.1.0"
source = { virtual = "." }
dependencies = [
    { name = "boto3" },
    { name = "pyarrow" },
]

[package.metadata]
requires-dist = [
    { name = "boto3", specifier = ">=1.36.21" },
    { name = "pyarrow", specifier = ">=19.0.0" },
]

[[package]]
name = "s3transfer"
version = "0.11.2"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "botocore" },
]
sdist = { url = "https://files.pythonhosted.org/packages/62/45/2323b5928f86fd29f9afdcef4659f68fa73eaa5356912b774227f5cf46b5/s3transfer-0.11.2.tar.gz", hash = "sha256:3b39185cb72f5acc77db1a58b6e25b977f28d20496b6e58d6813d75f464d632f", size = 147885 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/1b/ac/e7dc469e49048dc57f62e0c555d2ee3117fa30813d2a1a2962cce3a2a82a/s3transfer-0.11.2-py3-none-any.whl", hash = "sha256:be6ecb39fadd986ef1701097771f87e4d2f821f27f6071c872143884d2950fbc", size = 84151 },
]

[[package]]
name = "six"
version = "1.17.0"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/94/e7/b2c673351809dca68a0e064b6af791aa332cf192da575fd474ed7d6f16a2/six-1.17.0.tar.gz", hash = "sha256:ff70335d468e7eb6ec65b95b99d3a2836546063f63acc5171de367e834932a81", size = 34031 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/b7/ce/149a00dd41f10bc29e5921b496af8b574d8413afcd5e30dfa0ed46c2cc5e/six-1.17.0-py2.py3-none-any.whl", hash = "sha256:4721f391ed90541fddacab5acf947aa0d3dc7d27b2e1e8eda2be8970586c3274", size = 11050 },
]

[[package]]
name = "urllib3"
version = "2.3.0"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/aa/63/e53da845320b757bf29ef6a9062f5c669fe997973f966045cb019c3f4b66/urllib3-2.3.0.tar.gz", hash = "sha256:f8c5449b3cf0861679ce7e0503c7b44b5ec981bec0d1d3795a07f1ba96f0204d", size = 307268 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/c8/19/4ec628951a74043532ca2cf5d97b7b14863931476d117c471e8e2b1eb39f/urllib3-2.3.0-py3-none-any.whl", hash = "sha256:1cee9ad369867bfdbbb48b7dd50374c0967a0bb7710050facf0dd6911440e3df", size = 128369 },
]
```

### Platform

macOS 14 ARM64

### Version

uv 0.5.30 (ddbc6e315 2025-02-10)

### Python version

3.11

---

_Label `bug` added by @oresttokovenko on 2025-02-17 20:17_

---

_Comment by @oresttokovenko on 2025-02-17 20:23_

not sure why but `no-cache` fixed it

---

_Closed by @oresttokovenko on 2025-02-17 20:23_

---

_Comment by @charliermarsh on 2025-02-17 20:23_

Very strange, thanks for following up... It looks like `pyspark` doesn't publish a built distribution, so I was suspecting that we were building from source, and that was taking a while? If you're able to reproduce it consistently, let me know and we can re-open.

---
