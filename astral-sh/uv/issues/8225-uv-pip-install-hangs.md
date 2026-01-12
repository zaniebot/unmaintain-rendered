```yaml
number: 8225
title: "`uv pip install` hangs..."
type: issue
state: closed
author: milkpirate
labels:
  - needs-mre
assignees: []
created_at: 2024-10-15T17:24:41Z
updated_at: 2025-04-27T15:47:07Z
url: https://github.com/astral-sh/uv/issues/8225
synced_at: 2026-01-12T15:59:22Z
```

# `uv pip install` hangs...

---

_@milkpirate_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hey,

I am new to `uv` but really would like to use it because of the install speed.

I have the following in a kaniko build:
```
INFO[0074] Running: [/bin/bash -o pipefail -c uv pip install --system --verbose --no-cache-dir .   && rm -vr uv.lock pyproject.toml] 
DEBUG uv 0.4.21
DEBUG Searching for default Python interpreter in system path
DEBUG Found `cpython-3.13.0-linux-x86_64-gnu` at `/usr/local/bin/python` (search path)
Using Python 3.13.0 environment at /usr/local
DEBUG Acquired lock for `/usr/local`
DEBUG At least one requirement is not satisfied: file:///
DEBUG Using request timeout of 30s
DEBUG Found PEP 621 metadata for / in `pyproject.toml` (dsa-characters)
DEBUG Found static `pyproject.toml` for: dsa-characters @ file:///
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13.0
...
DEBUG Released lock at `/tmp/.tmpVGhWbP/sdists-v4/pypi/docopt/0.6.2/.lock`
DEBUG warning: no files found matching '*.py' under directory 'pyautogui'
DEBUG running build
DEBUG running build_py
DEBUG creating build/lib/pyscreeze
DEBUG copying pyscreeze/__init__.py -> build/lib/pyscreeze
DEBUG installing to build/bdist.linux-x86_64/wheel
DEBUG running install
DEBUG running install_lib
DEBUG creating build/bdist.linux-x86_64/wheel
DEBUG creating build/bdist.linux-x86_64/wheel/pyscreeze
DEBUG copying build/lib/pyscreeze/__init__.py -> build/bdist.linux-x86_64/wheel/./pyscreeze
DEBUG running install_egg_info
DEBUG running egg_info
DEBUG writing PyScreeze.egg-info/PKG-INFO
DEBUG writing dependency_links to PyScreeze.egg-info/dependency_links.txt
DEBUG writing requirements to PyScreeze.egg-info/requires.txt
DEBUG writing top-level names to PyScreeze.egg-info/top_level.txt
DEBUG reading manifest file 'PyScreeze.egg-info/SOURCES.txt'
DEBUG reading manifest template 'MANIFEST.in'
DEBUG adding license file 'LICENSE.txt'
DEBUG adding license file 'AUTHORS.txt'
DEBUG writing manifest file 'PyScreeze.egg-info/SOURCES.txt'
DEBUG Copying PyScreeze.egg-info to build/bdist.linux-x86_64/wheel/./PyScreeze-1.0.1-py3.13.egg-info
DEBUG running install_scripts
DEBUG creating build/bdist.linux-x86_64/wheel/PyScreeze-1.0.1.dist-info/WHEEL
DEBUG creating '/tmp/.tmpVGhWbP/sdists-v4/pypi/pyscreeze/1.0.1/mihkJMXHDMQ04-S5gohOR/.tmpLAEb6q/.tmp-ixmbgzr5/PyScreeze-1.0.1-py3-none-any.whl' and adding 'build/bdist.linux-x86_64/wheel' to it
DEBUG adding 'pyscreeze/__init__.py'
DEBUG adding 'PyScreeze-1.0.1.dist-info/AUTHORS.txt'
DEBUG adding 'PyScreeze-1.0.1.dist-info/LICENSE.txt'
DEBUG adding 'PyScreeze-1.0.1.dist-info/METADATA'
DEBUG adding 'PyScreeze-1.0.1.dist-info/WHEEL'
DEBUG adding 'PyScreeze-1.0.1.dist-info/top_level.txt'
DEBUG adding 'PyScreeze-1.0.1.dist-info/RECORD'
DEBUG removing build/bdist.linux-x86_64/wheel
DEBUG Finished building: timeout-decorator==0.5.0
DEBUG Released lock at `/tmp/.tmpVGhWbP/sdists-v4/pypi/timeout-decorator/0.5.0/.lock`
DEBUG Finished building: pyscreeze==1.0.1
DEBUG Released lock at `/tmp/.tmpVGhWbP/sdists-v4/pypi/pyscreeze/1.0.1/.lock`
<hangs here>
```
I have seen there are a lot of issues reporting about hanging but I havent seen one, that seemed related to mine?

Could you help me out on that?

---

_Comment by @zanieb on 2024-10-15 17:26_

Can you share logs with `RUST_LOG=uv=trace` set?

---

_Comment by @milkpirate on 2024-10-15 17:37_

Ok, does not look much different:
```
# RUST_LOG=uv=trace uv pip install --system --verbose --no-cache-dir .
DEBUG uv 0.4.21
DEBUG Searching for default Python interpreter in system path
TRACE Searching PATH for executables: python, python3
TRACE Checking `PATH` directory for interpreters: /usr/local/bin
TRACE Found possible Python executable: /usr/local/bin/python
TRACE Querying interpreter executable at /usr/local/bin/python
DEBUG Found `cpython-3.13.0-linux-x86_64-gnu` at `/usr/local/bin/python` (search path)
Using Python 3.13.0 environment at /usr/local
TRACE Checking lock for `/usr/local` at `/tmp/uv-1b696e695b7c17a7.lock`
DEBUG Acquired lock for `/usr/local`
DEBUG At least one requirement is not satisfied: file:///
DEBUG Using request timeout of 30s
DEBUG Found PEP 621 metadata for / in `pyproject.toml` (dsa-characters)
TRACE Performing lookahead for dsa-characters @ file:///
DEBUG Found static `pyproject.toml` for: dsa-characters @ file:///
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13.0
DEBUG Adding direct dependency: dsa-characters*
DEBUG Searching for a compatible version of dsa-characters @ file:/// (*)
DEBUG Adding transitive dependency for dsa-characters==0.1.0: docopt>=0.6.2
DEBUG Adding transitive dependency for dsa-characters==0.1.0: opencv-python>=4.10.0.84
DEBUG Adding transitive dependency for dsa-characters==0.1.0: pillow>=10.4.0
DEBUG Adding transitive dependency for dsa-characters==0.1.0: pyautogui>=0.9.54
DEBUG Adding transitive dependency for dsa-characters==0.1.0: structlog>=24.4.0
DEBUG Adding transitive dependency for dsa-characters==0.1.0: timeout-decorator>=0.5.0
TRACE Fetching metadata for docopt from https://pypi.org/simple/docopt/
TRACE Fetching metadata for opencv-python from https://pypi.org/simple/opencv-python/
TRACE No cache entry exists for /tmp/.tmp9cbcox/simple-v13/pypi/opencv-python.rkyv
DEBUG No cache entry for: https://pypi.org/simple/opencv-python/
TRACE Sending fresh GET request for https://pypi.org/simple/opencv-python/
TRACE Handling request for https://pypi.org/simple/opencv-python/
TRACE Request for https://pypi.org/simple/opencv-python/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/opencv-python/
TRACE Attempting unauthenticated request for https://pypi.org/simple/opencv-python/
TRACE Fetching metadata for pillow from https://pypi.org/simple/pillow/
TRACE Fetching metadata for pyautogui from https://pypi.org/simple/pyautogui/
TRACE Fetching metadata for structlog from https://pypi.org/simple/structlog/
TRACE Fetching metadata for timeout-decorator from https://pypi.org/simple/timeout-decorator/
TRACE No cache entry exists for /tmp/.tmp9cbcox/simple-v13/pypi/docopt.rkyv
DEBUG No cache entry for: https://pypi.org/simple/docopt/
TRACE Sending fresh GET request for https://pypi.org/simple/docopt/
TRACE Handling request for https://pypi.org/simple/docopt/
TRACE Request for https://pypi.org/simple/docopt/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/docopt/
TRACE Attempting unauthenticated request for https://pypi.org/simple/docopt/
TRACE No cache entry exists for /tmp/.tmp9cbcox/simple-v13/pypi/pillow.rkyv
DEBUG No cache entry for: https://pypi.org/simple/pillow/
TRACE Sending fresh GET request for https://pypi.org/simple/pillow/
TRACE Handling request for https://pypi.org/simple/pillow/
...
DEBUG Finished building: pytweening==1.2.0
DEBUG Released lock at `/tmp/.tmpF4GIZu/sdists-v4/pypi/pytweening/1.2.0/.lock`
DEBUG running build
DEBUG running build_py
DEBUG creating build/lib/pyscreeze
DEBUG copying pyscreeze/__init__.py -> build/lib/pyscreeze
DEBUG installing to build/bdist.linux-x86_64/wheel
DEBUG running install
DEBUG running install_lib
DEBUG creating build/bdist.linux-x86_64/wheel
DEBUG creating build/bdist.linux-x86_64/wheel/pyscreeze
DEBUG copying build/lib/pyscreeze/__init__.py -> build/bdist.linux-x86_64/wheel/./pyscreeze
DEBUG running install_egg_info
DEBUG running egg_info
DEBUG writing PyScreeze.egg-info/PKG-INFO
DEBUG writing dependency_links to PyScreeze.egg-info/dependency_links.txt
DEBUG writing requirements to PyScreeze.egg-info/requires.txt
DEBUG writing top-level names to PyScreeze.egg-info/top_level.txt
DEBUG reading manifest file 'PyScreeze.egg-info/SOURCES.txt'
DEBUG reading manifest template 'MANIFEST.in'
DEBUG warning: no files found matching '*.py' under directory 'pyautogui'
DEBUG adding license file 'LICENSE.txt'
DEBUG adding license file 'AUTHORS.txt'
DEBUG writing manifest file 'PyScreeze.egg-info/SOURCES.txt'
DEBUG Copying PyScreeze.egg-info to build/bdist.linux-x86_64/wheel/./PyScreeze-1.0.1-py3.13.egg-info
DEBUG running install_scripts
DEBUG creating build/bdist.linux-x86_64/wheel/PyScreeze-1.0.1.dist-info/WHEEL
DEBUG creating '/tmp/.tmpF4GIZu/sdists-v4/pypi/pyscreeze/1.0.1/9iQriEiX2c_XcFPu2jtYQ/.tmpPcx3bF/.tmp-v1y9u2ek/PyScreeze-1.0.1-py3-none-any.whl' and adding 'build/bdist.linux-x86_64/wheel' to it
DEBUG adding 'pyscreeze/__init__.py'
DEBUG adding 'PyScreeze-1.0.1.dist-info/AUTHORS.txt'
DEBUG adding 'PyScreeze-1.0.1.dist-info/LICENSE.txt'
DEBUG adding 'PyScreeze-1.0.1.dist-info/METADATA'
DEBUG adding 'PyScreeze-1.0.1.dist-info/WHEEL'
DEBUG adding 'PyScreeze-1.0.1.dist-info/top_level.txt'
DEBUG adding 'PyScreeze-1.0.1.dist-info/RECORD'
DEBUG removing build/bdist.linux-x86_64/wheel
DEBUG Finished building: pyscreeze==1.0.1
DEBUG Released lock at `/tmp/.tmpF4GIZu/sdists-v4/pypi/pyscreeze/1.0.1/.lock`
<hangs>
```

---

_Comment by @milkpirate on 2024-10-15 17:44_

github does not let me upload these extensions:

`pyproject.toml`:
```toml
[project]
name = "dsa-characters"
version = "0.1.0"
description = "DSA character sheet genertor (via Optolit)"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "docopt>=0.6.2",
    "opencv-python>=4.10.0.84",
    "pillow>=10.4.0",
    "pyautogui>=0.9.54",
    "structlog>=24.4.0",
    "timeout-decorator>=0.5.0",
]

[tool.uv]
dev-dependencies = [
    "pytest>=8.3.3",
    "pytest-cov>=5.0.0",
    "pytest-html>=4.1.1",
]
```
`uv.lock`:
```
version = 1
requires-python = ">=3.12"
resolution-markers = [
    "platform_system == 'Darwin'",
    "platform_machine == 'aarch64' and platform_system == 'Linux'",
    "(platform_machine != 'aarch64' and platform_system != 'Darwin') or (platform_system != 'Darwin' and platform_system != 'Linux')",
]

[[package]]
name = "colorama"
version = "0.4.6"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/d8/53/6f443c9a4a8358a93a6792e2acffb9d9d5cb0a5cfd8802644b7b1c9a02e4/colorama-0.4.6.tar.gz", hash = "sha256:08695f5cb7ed6e0531a20572697297273c47b8cae5a63ffc6d6ed5c201be6e44", size = 27697 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl", hash = "sha256:4f1d9991f5acc0ca119f9d443620b77f9d6b33703e51011c16baf57afb285fc6", size = 25335 },
]

[[package]]
name = "coverage"
version = "7.6.2"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/9a/60/e781e8302e7b28f21ce06e30af077f856aa2cb4cf2253287dae9a593d509/coverage-7.6.2.tar.gz", hash = "sha256:a5f81e68aa62bc0cfca04f7b19eaa8f9c826b53fc82ab9e2121976dc74f131f3", size = 797872 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/3f/ac/1cca5ed5cf512a71cdd6e3afb75a5ef196f7ef9772be9192dadaaa5cfc1c/coverage-7.6.2-cp312-cp312-macosx_10_13_x86_64.whl", hash = "sha256:ebc94fadbd4a3f4215993326a6a00e47d79889391f5659bf310f55fe5d9f581c", size = 206856 },
    { url = "https://files.pythonhosted.org/packages/e4/58/030354d250f107a95e7aca24c7fd238709a3c7df3083cb206368798e637a/coverage-7.6.2-cp312-cp312-macosx_11_0_arm64.whl", hash = "sha256:9681516288e3dcf0aa7c26231178cc0be6cac9705cac06709f2353c5b406cfea", size = 207098 },
    { url = "https://files.pythonhosted.org/packages/03/df/5f2cd6048d44a54bb5f58f8ece4efbc5b686ed49f8bd8dbf41eb2a6a687f/coverage-7.6.2-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:8d9c5d13927d77af4fbe453953810db766f75401e764727e73a6ee4f82527b3e", size = 240109 },
    { url = "https://files.pythonhosted.org/packages/d3/18/7c53887643d921faa95529643b1b33e60ebba30ab835c8b5abd4e54d946b/coverage-7.6.2-cp312-cp312-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:b92f9ca04b3e719d69b02dc4a69debb795af84cb7afd09c5eb5d54b4a1ae2191", size = 237141 },
    { url = "https://files.pythonhosted.org/packages/d2/79/339bdf597d128374e6150c089b37436ba694585d769cabf6d5abd73a1365/coverage-7.6.2-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:0ff2ef83d6d0b527b5c9dad73819b24a2f76fdddcfd6c4e7a4d7e73ecb0656b4", size = 239210 },
    { url = "https://files.pythonhosted.org/packages/a9/62/7310c6de2bcb8a42f91094d41f0d4793ccda5a54621be3db76a156556cf2/coverage-7.6.2-cp312-cp312-musllinux_1_2_aarch64.whl", hash = "sha256:47ccb6e99a3031ffbbd6e7cc041e70770b4fe405370c66a54dbf26a500ded80b", size = 238698 },
    { url = "https://files.pythonhosted.org/packages/f2/cb/ccb23c084d7f581f770dc7ed547dc5b50763334ad6ce26087a9ad0b5b26d/coverage-7.6.2-cp312-cp312-musllinux_1_2_i686.whl", hash = "sha256:a867d26f06bcd047ef716175b2696b315cb7571ccb951006d61ca80bbc356e9e", size = 237000 },
    { url = "https://files.pythonhosted.org/packages/e7/ab/58de9e2f94e4dc91b84d6e2705aa1e9d5447a2669fe113b4bbce6d2224a1/coverage-7.6.2-cp312-cp312-musllinux_1_2_x86_64.whl", hash = "sha256:cdfcf2e914e2ba653101157458afd0ad92a16731eeba9a611b5cbb3e7124e74b", size = 238666 },
    { url = "https://files.pythonhosted.org/packages/6c/dc/8be87b9ed5dbd4892b603f41088b41982768e928734e5bdce67d2ddd460a/coverage-7.6.2-cp312-cp312-win32.whl", hash = "sha256:f9035695dadfb397bee9eeaf1dc7fbeda483bf7664a7397a629846800ce6e276", size = 209489 },
    { url = "https://files.pythonhosted.org/packages/64/3a/3f44e55273a58bfb39b87ad76541bbb81d14de916b034fdb39971cc99ffe/coverage-7.6.2-cp312-cp312-win_amd64.whl", hash = "sha256:5ed69befa9a9fc796fe015a7040c9398722d6b97df73a6b608e9e275fa0932b0", size = 210270 },
    { url = "https://files.pythonhosted.org/packages/ae/99/c9676a75b57438a19c5174dfcf39798b42728ad56650497286379dc0c2c3/coverage-7.6.2-cp313-cp313-macosx_10_13_x86_64.whl", hash = "sha256:4eea60c79d36a8f39475b1af887663bc3ae4f31289cd216f514ce18d5938df40", size = 206888 },
    { url = "https://files.pythonhosted.org/packages/e0/de/820ecb42e892049c5f384430e98b35b899da3451dd0cdb2f867baf26abfa/coverage-7.6.2-cp313-cp313-macosx_11_0_arm64.whl", hash = "sha256:aa68a6cdbe1bc6793a9dbfc38302c11599bbe1837392ae9b1d238b9ef3dafcf1", size = 207142 },
    { url = "https://files.pythonhosted.org/packages/dd/59/81fc7ad855d65eeb68fe9e7809cbb339946adb07be7ac32d3fc24dc17bd7/coverage-7.6.2-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:3ec528ae69f0a139690fad6deac8a7d33629fa61ccce693fdd07ddf7e9931fba", size = 239658 },
    { url = "https://files.pythonhosted.org/packages/cd/a7/865de3eb9e78ffbf7afd92f86d2580b18edfb6f0481bd3c39b205e05a762/coverage-7.6.2-cp313-cp313-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:ed5ac02126f74d190fa2cc14a9eb2a5d9837d5863920fa472b02eb1595cdc925", size = 236802 },
    { url = "https://files.pythonhosted.org/packages/36/94/3b8f3abf88b7c451f97fd14c98f536bcee364e74250d928d57cc97c38ddd/coverage-7.6.2-cp313-cp313-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:21c0ea0d4db8a36b275cb6fb2437a3715697a4ba3cb7b918d3525cc75f726304", size = 238793 },
    { url = "https://files.pythonhosted.org/packages/d5/4b/57f95e41a10525002f524f3dbd577a3a9871d67998f8a8eb192fe697dc7b/coverage-7.6.2-cp313-cp313-musllinux_1_2_aarch64.whl", hash = "sha256:35a51598f29b2a19e26d0908bd196f771a9b1c5d9a07bf20be0adf28f1ad4f77", size = 238455 },
    { url = "https://files.pythonhosted.org/packages/99/c9/9fbe5b841628e1d9030c8044844afef4f4735586289eb9237eeb5b97f0d7/coverage-7.6.2-cp313-cp313-musllinux_1_2_i686.whl", hash = "sha256:c9192925acc33e146864b8cf037e2ed32a91fdf7644ae875f5d46cd2ef086a5f", size = 236538 },
    { url = "https://files.pythonhosted.org/packages/43/0d/2200a0d447e30de94d48e4851c04d8dce37340815e7eda27457a7043c037/coverage-7.6.2-cp313-cp313-musllinux_1_2_x86_64.whl", hash = "sha256:bf4eeecc9e10f5403ec06138978235af79c9a79af494eb6b1d60a50b49ed2869", size = 238383 },
    { url = "https://files.pythonhosted.org/packages/ec/8a/106c66faafb4a87002b698769d6de3c4db0b6c29a7aeb72de13b893c333e/coverage-7.6.2-cp313-cp313-win32.whl", hash = "sha256:e4ee15b267d2dad3e8759ca441ad450c334f3733304c55210c2a44516e8d5530", size = 209551 },
    { url = "https://files.pythonhosted.org/packages/c4/f5/1b39e2faaf5b9cc7eed568c444df5991ce7ff7138e2e735a6801be1bdadb/coverage-7.6.2-cp313-cp313-win_amd64.whl", hash = "sha256:c71965d1ced48bf97aab79fad56df82c566b4c498ffc09c2094605727c4b7e36", size = 210282 },
    { url = "https://files.pythonhosted.org/packages/79/a3/8dd4e6c09f5286094cd6c7edb115b3fbf06ad8304d45431722a4e3bc2508/coverage-7.6.2-cp313-cp313t-macosx_10_13_x86_64.whl", hash = "sha256:7571e8bbecc6ac066256f9de40365ff833553e2e0c0c004f4482facb131820ef", size = 207629 },
    { url = "https://files.pythonhosted.org/packages/8e/db/a9aa7009bbdc570a235e1ac781c0a83aa323cac6db8f8f13c2127b110978/coverage-7.6.2-cp313-cp313t-macosx_11_0_arm64.whl", hash = "sha256:078a87519057dacb5d77e333f740708ec2a8f768655f1db07f8dfd28d7a005f0", size = 207902 },
    { url = "https://files.pythonhosted.org/packages/54/08/d0962be62d4335599ca2ff3a48bb68c9bfb80df74e28ca689ff5f392087b/coverage-7.6.2-cp313-cp313t-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:1e5e92e3e84a8718d2de36cd8387459cba9a4508337b8c5f450ce42b87a9e760", size = 250617 },
    { url = "https://files.pythonhosted.org/packages/a5/a2/158570aff1dd88b661a6c11281cbb190e8696e77798b4b2e47c74bfb2f39/coverage-7.6.2-cp313-cp313t-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:ebabdf1c76593a09ee18c1a06cd3022919861365219ea3aca0247ededf6facd6", size = 246334 },
    { url = "https://files.pythonhosted.org/packages/aa/fe/b00428cca325b6585ca77422e4f64d7d86a225b14664b98682ea501efb57/coverage-7.6.2-cp313-cp313t-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:12179eb0575b8900912711688e45474f04ab3934aaa7b624dea7b3c511ecc90f", size = 248692 },
    { url = "https://files.pythonhosted.org/packages/30/21/0a15fefc13039450bc45e7159f3add92489f004555eb7dab9c7ad4365dd0/coverage-7.6.2-cp313-cp313t-musllinux_1_2_aarch64.whl", hash = "sha256:39d3b964abfe1519b9d313ab28abf1d02faea26cd14b27f5283849bf59479ff5", size = 248188 },
    { url = "https://files.pythonhosted.org/packages/de/b8/5c093526046a8450a7a3d62ad09517cf38e638f6b3ee9433dd6a73360501/coverage-7.6.2-cp313-cp313t-musllinux_1_2_i686.whl", hash = "sha256:84c4315577f7cd511d6250ffd0f695c825efe729f4205c0340f7004eda51191f", size = 246072 },
    { url = "https://files.pythonhosted.org/packages/1e/8b/542b607d2cff56e5a90a6948f5a9040b693761d2be2d3c3bf88957b02361/coverage-7.6.2-cp313-cp313t-musllinux_1_2_x86_64.whl", hash = "sha256:ff797320dcbff57caa6b2301c3913784a010e13b1f6cf4ab3f563f3c5e7919db", size = 247354 },
    { url = "https://files.pythonhosted.org/packages/95/82/2e9111aa5e59f42b332d387f64e3205c2263518d1e660154d0c9fc54390e/coverage-7.6.2-cp313-cp313t-win32.whl", hash = "sha256:2b636a301e53964550e2f3094484fa5a96e699db318d65398cfba438c5c92171", size = 210194 },
    { url = "https://files.pythonhosted.org/packages/9d/46/aabe4305cfc57cab4865f788ceceef746c422469720c32ed7a5b44e20f5e/coverage-7.6.2-cp313-cp313t-win_amd64.whl", hash = "sha256:d03a060ac1a08e10589c27d509bbdb35b65f2d7f3f8d81cf2fa199877c7bc58a", size = 211346 },
]

[[package]]
name = "docopt"
version = "0.6.2"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/a2/55/8f8cab2afd404cf578136ef2cc5dfb50baa1761b68c9da1fb1e4eed343c9/docopt-0.6.2.tar.gz", hash = "sha256:49b3a825280bd66b3aa83585ef59c4a8c82f2c8a522dbe754a8bc8d08c85c491", size = 25901 }

[[package]]
name = "dsa-characters"
version = "0.1.0"
source = { virtual = "." }
dependencies = [
    { name = "docopt", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "opencv-python", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "pillow", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "pyautogui", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "structlog", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "timeout-decorator", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
]

[package.dev-dependencies]
dev = [
    { name = "pytest", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "pytest-cov", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "pytest-html", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
]

[package.metadata]
requires-dist = [
    { name = "docopt", specifier = ">=0.6.2" },
    { name = "opencv-python", specifier = ">=4.10.0.84" },
    { name = "pillow", specifier = ">=10.4.0" },
    { name = "pyautogui", specifier = ">=0.9.54" },
    { name = "structlog", specifier = ">=24.4.0" },
    { name = "timeout-decorator", specifier = ">=0.5.0" },
]

[package.metadata.requires-dev]
dev = [
    { name = "pytest", specifier = ">=8.3.3" },
    { name = "pytest-cov", specifier = ">=5.0.0" },
    { name = "pytest-html", specifier = ">=4.1.1" },
]

[[package]]
name = "iniconfig"
version = "2.0.0"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/d7/4b/cbd8e699e64a6f16ca3a8220661b5f83792b3017d0f79807cb8708d33913/iniconfig-2.0.0.tar.gz", hash = "sha256:2d91e135bf72d31a410b17c16da610a82cb55f6b0477d1a902134b24a455b8b3", size = 4646 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/ef/a6/62565a6e1cf69e10f5727360368e451d4b7f58beeac6173dc9db836a5b46/iniconfig-2.0.0-py3-none-any.whl", hash = "sha256:b6a85871a79d2e3b22d2d1b94ac2824226a63c6b741c88f7ae975f18b6778374", size = 5892 },
]

[[package]]
name = "jinja2"
version = "3.1.4"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "markupsafe", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
]
sdist = { url = "https://files.pythonhosted.org/packages/ed/55/39036716d19cab0747a5020fc7e907f362fbf48c984b14e62127f7e68e5d/jinja2-3.1.4.tar.gz", hash = "sha256:4a3aee7acbbe7303aede8e9648d13b8bf88a429282aa6122a993f0ac800cb369", size = 240245 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/31/80/3a54838c3fb461f6fec263ebf3a3a41771bd05190238de3486aae8540c36/jinja2-3.1.4-py3-none-any.whl", hash = "sha256:bc5dd2abb727a5319567b7a813e6a2e7318c39f4f487cfe6c89c6f9c7d25197d", size = 133271 },
]

[[package]]
name = "markupsafe"
version = "3.0.1"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/b4/d2/38ff920762f2247c3af5cbbbbc40756f575d9692d381d7c520f45deb9b8f/markupsafe-3.0.1.tar.gz", hash = "sha256:3e683ee4f5d0fa2dde4db77ed8dd8a876686e3fc417655c2ece9a90576905344", size = 20249 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/45/6d/72ed58d42a12bd9fc288dbff6dd8d03ea973a232ac0538d7f88d105b5251/MarkupSafe-3.0.1-cp312-cp312-macosx_10_13_universal2.whl", hash = "sha256:8ae369e84466aa70f3154ee23c1451fda10a8ee1b63923ce76667e3077f2b0c4", size = 14322 },
    { url = "https://files.pythonhosted.org/packages/86/f5/241238f89cdd6461ac9f521af8389f9a48fab97e4f315c69e9e0d52bc919/MarkupSafe-3.0.1-cp312-cp312-macosx_11_0_arm64.whl", hash = "sha256:40f1e10d51c92859765522cbd79c5c8989f40f0419614bcdc5015e7b6bf97fc5", size = 12380 },
    { url = "https://files.pythonhosted.org/packages/27/94/79751928bca5841416d8ca02e22198672e021d5c7120338e2a6e3771f8fc/MarkupSafe-3.0.1-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:5a4cb365cb49b750bdb60b846b0c0bc49ed62e59a76635095a179d440540c346", size = 24099 },
    { url = "https://files.pythonhosted.org/packages/10/6e/1b8070bbfc467429c7983cd5ffd4ec57e1d501763d974c7caaa0a9a79f4c/MarkupSafe-3.0.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:ee3941769bd2522fe39222206f6dd97ae83c442a94c90f2b7a25d847d40f4729", size = 23249 },
    { url = "https://files.pythonhosted.org/packages/66/50/9389ae6cdff78d7481a2a2641830b5eb1d1f62177550e73355a810a889c9/MarkupSafe-3.0.1-cp312-cp312-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:62fada2c942702ef8952754abfc1a9f7658a4d5460fabe95ac7ec2cbe0d02abc", size = 23149 },
    { url = "https://files.pythonhosted.org/packages/16/02/5dddff5366fde47133186efb847fa88bddef85914bbe623e25cfeccb3517/MarkupSafe-3.0.1-cp312-cp312-musllinux_1_2_aarch64.whl", hash = "sha256:4c2d64fdba74ad16138300815cfdc6ab2f4647e23ced81f59e940d7d4a1469d9", size = 23864 },
    { url = "https://files.pythonhosted.org/packages/f3/f1/700ee6655561cfda986e03f7afc309e3738918551afa7dedd99225586227/MarkupSafe-3.0.1-cp312-cp312-musllinux_1_2_i686.whl", hash = "sha256:fb532dd9900381d2e8f48172ddc5a59db4c445a11b9fab40b3b786da40d3b56b", size = 23440 },
    { url = "https://files.pythonhosted.org/packages/fb/3e/d26623ac7f16709823b4c80e0b4a1c9196eeb46182a6c1d47b5e0c8434f4/MarkupSafe-3.0.1-cp312-cp312-musllinux_1_2_x86_64.whl", hash = "sha256:0f84af7e813784feb4d5e4ff7db633aba6c8ca64a833f61d8e4eade234ef0c38", size = 23610 },
    { url = "https://files.pythonhosted.org/packages/51/04/1f8da0810c39cb9fcff96b6baed62272c97065e9cf11471965a161439e20/MarkupSafe-3.0.1-cp312-cp312-win32.whl", hash = "sha256:cbf445eb5628981a80f54087f9acdbf84f9b7d862756110d172993b9a5ae81aa", size = 15113 },
    { url = "https://files.pythonhosted.org/packages/eb/24/a36dc37365bdd358b1e583cc40475593e36ab02cb7da6b3d0b9c05b0da7a/MarkupSafe-3.0.1-cp312-cp312-win_amd64.whl", hash = "sha256:a10860e00ded1dd0a65b83e717af28845bb7bd16d8ace40fe5531491de76b79f", size = 15611 },
    { url = "https://files.pythonhosted.org/packages/b1/60/4572a8aa1beccbc24b133aa0670781a5d2697f4fa3fecf0a87b46383174b/MarkupSafe-3.0.1-cp313-cp313-macosx_10_13_universal2.whl", hash = "sha256:e81c52638315ff4ac1b533d427f50bc0afc746deb949210bc85f05d4f15fd772", size = 14325 },
    { url = "https://files.pythonhosted.org/packages/38/42/849915b99a765ec104bfd07ee933de5fc9c58fa9570efa7db81717f495d8/MarkupSafe-3.0.1-cp313-cp313-macosx_11_0_arm64.whl", hash = "sha256:312387403cd40699ab91d50735ea7a507b788091c416dd007eac54434aee51da", size = 12373 },
    { url = "https://files.pythonhosted.org/packages/ef/82/4caaebd963c6d60b28e4445f38841d24f8b49bc10594a09956c9d73bfc08/MarkupSafe-3.0.1-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:2ae99f31f47d849758a687102afdd05bd3d3ff7dbab0a8f1587981b58a76152a", size = 24059 },
    { url = "https://files.pythonhosted.org/packages/20/15/6b319be2f79fcfa3173f479d69f4e950b5c9b642db4f22cf73ae5ade745f/MarkupSafe-3.0.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:c97ff7fedf56d86bae92fa0a646ce1a0ec7509a7578e1ed238731ba13aabcd1c", size = 23211 },
    { url = "https://files.pythonhosted.org/packages/9d/3f/8963bdf4962feb2154475acb7dc350f04217b5e0be7763a39b432291e229/MarkupSafe-3.0.1-cp313-cp313-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:a7420ceda262dbb4b8d839a4ec63d61c261e4e77677ed7c66c99f4e7cb5030dd", size = 23095 },
    { url = "https://files.pythonhosted.org/packages/af/93/f770bc70953d32de0c6ce4bcb76271512123a1ead91aaef625a020c5bfaf/MarkupSafe-3.0.1-cp313-cp313-musllinux_1_2_aarch64.whl", hash = "sha256:45d42d132cff577c92bfba536aefcfea7e26efb975bd455db4e6602f5c9f45e7", size = 23901 },
    { url = "https://files.pythonhosted.org/packages/11/92/1e5a33aa0a1190161238628fb68eb1bc5e67b56a5c89f0636328704b463a/MarkupSafe-3.0.1-cp313-cp313-musllinux_1_2_i686.whl", hash = "sha256:4c8817557d0de9349109acb38b9dd570b03cc5014e8aabf1cbddc6e81005becd", size = 23463 },
    { url = "https://files.pythonhosted.org/packages/0d/fe/657efdfe385d2a3a701f2c4fcc9577c63c438aeefdd642d0d956c4ecd225/MarkupSafe-3.0.1-cp313-cp313-musllinux_1_2_x86_64.whl", hash = "sha256:6a54c43d3ec4cf2a39f4387ad044221c66a376e58c0d0e971d47c475ba79c6b5", size = 23569 },
    { url = "https://files.pythonhosted.org/packages/cf/24/587dea40304046ace60f846cedaebc0d33d967a3ce46c11395a10e7a78ba/MarkupSafe-3.0.1-cp313-cp313-win32.whl", hash = "sha256:c91b394f7601438ff79a4b93d16be92f216adb57d813a78be4446fe0f6bc2d8c", size = 15117 },
    { url = "https://files.pythonhosted.org/packages/32/8f/d8961d633f26a011b4fe054f3bfff52f673423b8c431553268741dfb089e/MarkupSafe-3.0.1-cp313-cp313-win_amd64.whl", hash = "sha256:fe32482b37b4b00c7a52a07211b479653b7fe4f22b2e481b9a9b099d8a430f2f", size = 15613 },
    { url = "https://files.pythonhosted.org/packages/9e/93/d6367ffbcd0c5c371370767f768eaa32af60bc411245b8517e383c6a2b12/MarkupSafe-3.0.1-cp313-cp313t-macosx_10_13_universal2.whl", hash = "sha256:17b2aea42a7280db02ac644db1d634ad47dcc96faf38ab304fe26ba2680d359a", size = 14563 },
    { url = "https://files.pythonhosted.org/packages/4a/37/f813c3835747dec08fe19ac9b9eced01fdf93a4b3e626521675dc7f423a9/MarkupSafe-3.0.1-cp313-cp313t-macosx_11_0_arm64.whl", hash = "sha256:852dc840f6d7c985603e60b5deaae1d89c56cb038b577f6b5b8c808c97580f1d", size = 12505 },
    { url = "https://files.pythonhosted.org/packages/72/bf/800b4d1580298ca91ccd6c95915bbd147142dad1b8cf91d57b93b28670dd/MarkupSafe-3.0.1-cp313-cp313t-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:0778de17cff1acaeccc3ff30cd99a3fd5c50fc58ad3d6c0e0c4c58092b859396", size = 25358 },
    { url = "https://files.pythonhosted.org/packages/fd/78/26e209abc8f0a379f031f0acc151231974e5b153d7eda5759d17d8f329f2/MarkupSafe-3.0.1-cp313-cp313t-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:800100d45176652ded796134277ecb13640c1a537cad3b8b53da45aa96330453", size = 23797 },
    { url = "https://files.pythonhosted.org/packages/09/e1/918496a9390891756efee818880e71c1bbaf587f4dc8ede3f3852357310a/MarkupSafe-3.0.1-cp313-cp313t-manylinux_2_5_i686.manylinux1_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:d06b24c686a34c86c8c1fba923181eae6b10565e4d80bdd7bc1c8e2f11247aa4", size = 23743 },
    { url = "https://files.pythonhosted.org/packages/cd/c6/26f576cd58d6c2decd9045e4e3f3c5dbc01ea6cb710916e7bbb6ebd95b6b/MarkupSafe-3.0.1-cp313-cp313t-musllinux_1_2_aarch64.whl", hash = "sha256:33d1c36b90e570ba7785dacd1faaf091203d9942bc036118fab8110a401eb1a8", size = 25076 },
    { url = "https://files.pythonhosted.org/packages/b5/fa/10b24fb3b0e15fe5389dc88ecc6226ede08297e0ba7130610efbe0cdfb27/MarkupSafe-3.0.1-cp313-cp313t-musllinux_1_2_i686.whl", hash = "sha256:beeebf760a9c1f4c07ef6a53465e8cfa776ea6a2021eda0d0417ec41043fe984", size = 24037 },
    { url = "https://files.pythonhosted.org/packages/c8/81/4b3f5537d9f6cc4f5c80d6c4b78af9a5247fd37b5aba95807b2cbc336b9a/MarkupSafe-3.0.1-cp313-cp313t-musllinux_1_2_x86_64.whl", hash = "sha256:bbde71a705f8e9e4c3e9e33db69341d040c827c7afa6789b14c6e16776074f5a", size = 24015 },
    { url = "https://files.pythonhosted.org/packages/5f/07/8e8dcecd53216c5e01a51e84c32a2bce166690ed19c184774b38cd41921d/MarkupSafe-3.0.1-cp313-cp313t-win32.whl", hash = "sha256:82b5dba6eb1bcc29cc305a18a3c5365d2af06ee71b123216416f7e20d2a84e5b", size = 15213 },
    { url = "https://files.pythonhosted.org/packages/0d/87/4c364e0f109eea2402079abecbe33fef4f347b551a11423d1f4e187ea497/MarkupSafe-3.0.1-cp313-cp313t-win_amd64.whl", hash = "sha256:730d86af59e0e43ce277bb83970530dd223bf7f2a838e086b50affa6ec5f9295", size = 15741 },
]

[[package]]
name = "mouseinfo"
version = "0.1.3"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "pyperclip", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "python3-xlib", marker = "(platform_system == 'Linux' and python_full_version >= '3.12') or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "rubicon-objc", marker = "platform_system == 'Darwin'" },
]
sdist = { url = "https://files.pythonhosted.org/packages/28/fa/b2ba8229b9381e8f6381c1dcae6f4159a7f72349e414ed19cfbbd1817173/MouseInfo-0.1.3.tar.gz", hash = "sha256:2c62fb8885062b8e520a3cce0a297c657adcc08c60952eb05bc8256ef6f7f6e7", size = 10850 }

[[package]]
name = "numpy"
version = "2.1.2"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/4b/d1/8a730ea07f4a37d94f9172f4ce1d81064b7a64766b460378be278952de75/numpy-2.1.2.tar.gz", hash = "sha256:13532a088217fa624c99b843eeb54640de23b3414b14aa66d023805eb731066c", size = 18878063 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/a0/7d/554a6838f37f3ada5a55f25173c619d556ae98092a6e01afb6e710501d70/numpy-2.1.2-cp312-cp312-macosx_10_13_x86_64.whl", hash = "sha256:d7bf0a4f9f15b32b5ba53147369e94296f5fffb783db5aacc1be15b4bf72f43b", size = 20848077 },
    { url = "https://files.pythonhosted.org/packages/b0/29/cb48a402ea879e645b16218718f3f7d9588a77d674a9dcf22e4c43487636/numpy-2.1.2-cp312-cp312-macosx_11_0_arm64.whl", hash = "sha256:b1d0fcae4f0949f215d4632be684a539859b295e2d0cb14f78ec231915d644db", size = 13493242 },
    { url = "https://files.pythonhosted.org/packages/56/44/f899b0581766c230da42f751b7b8896d096640b19b312164c267e48d36cb/numpy-2.1.2-cp312-cp312-macosx_14_0_arm64.whl", hash = "sha256:f751ed0a2f250541e19dfca9f1eafa31a392c71c832b6bb9e113b10d050cb0f1", size = 5089219 },
    { url = "https://files.pythonhosted.org/packages/79/8f/b987070d45161a7a4504afc67ed38544ed2c0ed5576263599a0402204a9c/numpy-2.1.2-cp312-cp312-macosx_14_0_x86_64.whl", hash = "sha256:bd33f82e95ba7ad632bc57837ee99dba3d7e006536200c4e9124089e1bf42426", size = 6620167 },
    { url = "https://files.pythonhosted.org/packages/c4/a7/af3329fda3c3ec31d9b650e42bbcd3422fc62a765cbb1405fde4177a0996/numpy-2.1.2-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:1b8cde4f11f0a975d1fd59373b32e2f5a562ade7cde4f85b7137f3de8fbb29a0", size = 13604905 },
    { url = "https://files.pythonhosted.org/packages/9b/b4/e3c7e6fab0f77fff6194afa173d1f2342073d91b1d3b4b30b17c3fb4407a/numpy-2.1.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:6d95f286b8244b3649b477ac066c6906fbb2905f8ac19b170e2175d3d799f4df", size = 16041825 },
    { url = "https://files.pythonhosted.org/packages/e9/50/6828e66a78aa03147c111f84d55f33ce2dde547cb578d6744a3b06a0124b/numpy-2.1.2-cp312-cp312-musllinux_1_1_x86_64.whl", hash = "sha256:ab4754d432e3ac42d33a269c8567413bdb541689b02d93788af4131018cbf366", size = 16409541 },
    { url = "https://files.pythonhosted.org/packages/bf/72/66af7916d9c3c6dbfbc8acdd4930c65461e1953374a2bc43d00f948f004a/numpy-2.1.2-cp312-cp312-musllinux_1_2_aarch64.whl", hash = "sha256:e585c8ae871fd38ac50598f4763d73ec5497b0de9a0ab4ef5b69f01c6a046142", size = 14081134 },
    { url = "https://files.pythonhosted.org/packages/dc/5a/59a67d84f33fe00ae74f0b5b69dd4f93a586a4aba7f7e19b54b2133db038/numpy-2.1.2-cp312-cp312-win32.whl", hash = "sha256:9c6c754df29ce6a89ed23afb25550d1c2d5fdb9901d9c67a16e0b16eaf7e2550", size = 6237784 },
    { url = "https://files.pythonhosted.org/packages/4c/79/73735a6a5dad6059c085f240a4e74c9270feccd2bc66e4d31b5ca01d329c/numpy-2.1.2-cp312-cp312-win_amd64.whl", hash = "sha256:456e3b11cb79ac9946c822a56346ec80275eaf2950314b249b512896c0d2505e", size = 12568254 },
    { url = "https://files.pythonhosted.org/packages/16/72/716fa1dbe92395a9a623d5049203ff8ddb0cfce65b9df9117c3696ccc011/numpy-2.1.2-cp313-cp313-macosx_10_13_x86_64.whl", hash = "sha256:a84498e0d0a1174f2b3ed769b67b656aa5460c92c9554039e11f20a05650f00d", size = 20834690 },
    { url = "https://files.pythonhosted.org/packages/1e/fb/3e85a39511586053b5c6a59a643879e376fae22230ebfef9cfabb0e032e2/numpy-2.1.2-cp313-cp313-macosx_11_0_arm64.whl", hash = "sha256:4d6ec0d4222e8ffdab1744da2560f07856421b367928026fb540e1945f2eeeaf", size = 13507474 },
    { url = "https://files.pythonhosted.org/packages/35/eb/5677556d9ba13436dab51e129f98d4829d95cd1b6bd0e199c14485a4bdb9/numpy-2.1.2-cp313-cp313-macosx_14_0_arm64.whl", hash = "sha256:259ec80d54999cc34cd1eb8ded513cb053c3bf4829152a2e00de2371bd406f5e", size = 5074742 },
    { url = "https://files.pythonhosted.org/packages/3e/c5/6c5ef5ba41b65a7e51bed50dbf3e1483eb578055633dd013e811a28e96a1/numpy-2.1.2-cp313-cp313-macosx_14_0_x86_64.whl", hash = "sha256:675c741d4739af2dc20cd6c6a5c4b7355c728167845e3c6b0e824e4e5d36a6c3", size = 6606787 },
    { url = "https://files.pythonhosted.org/packages/08/ac/f2f29dd4fd325b379c7dc932a0ebab22f0e031dbe80b2f6019b291a3a544/numpy-2.1.2-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:05b2d4e667895cc55e3ff2b56077e4c8a5604361fc21a042845ea3ad67465aa8", size = 13601333 },
    { url = "https://files.pythonhosted.org/packages/44/26/63f5f4e5089654dfb858f4892215ed968cd1a68e6f4a83f9961f84f855cb/numpy-2.1.2-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:43cca367bf94a14aca50b89e9bc2061683116cfe864e56740e083392f533ce7a", size = 16038090 },
    { url = "https://files.pythonhosted.org/packages/1d/21/015e0594de9c3a8d5edd24943d2bd23f102ec71aec026083f822f86497e2/numpy-2.1.2-cp313-cp313-musllinux_1_1_x86_64.whl", hash = "sha256:76322dcdb16fccf2ac56f99048af32259dcc488d9b7e25b51e5eca5147a3fb98", size = 16410865 },
    { url = "https://files.pythonhosted.org/packages/df/01/c1bcf9e6025d79077fbf3f3ee503b50aa7bfabfcd8f4b54f5829f4c00f3f/numpy-2.1.2-cp313-cp313-musllinux_1_2_aarch64.whl", hash = "sha256:32e16a03138cabe0cb28e1007ee82264296ac0983714094380b408097a418cfe", size = 14078077 },
    { url = "https://files.pythonhosted.org/packages/ba/06/db9d127d63bd11591770ba9f3d960f8041e0f895184b9351d4b1b5b56983/numpy-2.1.2-cp313-cp313-win32.whl", hash = "sha256:242b39d00e4944431a3cd2db2f5377e15b5785920421993770cddb89992c3f3a", size = 6234904 },
    { url = "https://files.pythonhosted.org/packages/a9/96/9f61f8f95b6e0ea0aa08633b704c75d1882bdcb331bdf8bfd63263b25b00/numpy-2.1.2-cp313-cp313-win_amd64.whl", hash = "sha256:f2ded8d9b6f68cc26f8425eda5d3877b47343e68ca23d0d0846f4d312ecaa445", size = 12561910 },
    { url = "https://files.pythonhosted.org/packages/36/b8/033f627821784a48e8f75c218033471eebbaacdd933f8979c79637a1b44b/numpy-2.1.2-cp313-cp313t-macosx_10_13_x86_64.whl", hash = "sha256:2ffef621c14ebb0188a8633348504a35c13680d6da93ab5cb86f4e54b7e922b5", size = 20857719 },
    { url = "https://files.pythonhosted.org/packages/96/46/af5726fde5b74ed83f2f17a73386d399319b7ed4d51279fb23b721d0816d/numpy-2.1.2-cp313-cp313t-macosx_11_0_arm64.whl", hash = "sha256:ad369ed238b1959dfbade9018a740fb9392c5ac4f9b5173f420bd4f37ba1f7a0", size = 13518826 },
    { url = "https://files.pythonhosted.org/packages/db/6e/8ce677edf36da1c4dae80afe5529f47690697eb55b4864673af260ccea7b/numpy-2.1.2-cp313-cp313t-macosx_14_0_arm64.whl", hash = "sha256:d82075752f40c0ddf57e6e02673a17f6cb0f8eb3f587f63ca1eaab5594da5b17", size = 5115036 },
    { url = "https://files.pythonhosted.org/packages/6a/ba/3cce44fb1b8438042c11847048812a776f75ee0e7070179c22e4cfbf420c/numpy-2.1.2-cp313-cp313t-macosx_14_0_x86_64.whl", hash = "sha256:1600068c262af1ca9580a527d43dc9d959b0b1d8e56f8a05d830eea39b7c8af6", size = 6628641 },
    { url = "https://files.pythonhosted.org/packages/59/c8/e722998720ccbd35ffbcf1d1b8ed0aa2304af88d3f1c38e06ebf983599b3/numpy-2.1.2-cp313-cp313t-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:a26ae94658d3ba3781d5e103ac07a876b3e9b29db53f68ed7df432fd033358a8", size = 13574803 },
    { url = "https://files.pythonhosted.org/packages/7c/8e/fc1fdd83a55476765329ac2913321c4aed5b082a7915095628c4ca30ea72/numpy-2.1.2-cp313-cp313t-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:13311c2db4c5f7609b462bc0f43d3c465424d25c626d95040f073e30f7570e35", size = 16021174 },
    { url = "https://files.pythonhosted.org/packages/2a/b6/a790742aa88067adb4bd6c89a946778c1417d4deaeafce3ca928f26d4c52/numpy-2.1.2-cp313-cp313t-musllinux_1_1_x86_64.whl", hash = "sha256:2abbf905a0b568706391ec6fa15161fad0fb5d8b68d73c461b3c1bab6064dd62", size = 16400117 },
    { url = "https://files.pythonhosted.org/packages/48/6f/129e3c17e3befe7fefdeaa6890f4c4df3f3cf0831aa053802c3862da67aa/numpy-2.1.2-cp313-cp313t-musllinux_1_2_aarch64.whl", hash = "sha256:ef444c57d664d35cac4e18c298c47d7b504c66b17c2ea91312e979fcfbdfb08a", size = 14066202 },
]

[[package]]
name = "opencv-python"
version = "4.10.0.84"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "numpy", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
]
sdist = { url = "https://files.pythonhosted.org/packages/4a/e7/b70a2d9ab205110d715906fc8ec83fbb00404aeb3a37a0654fdb68eb0c8c/opencv-python-4.10.0.84.tar.gz", hash = "sha256:72d234e4582e9658ffea8e9cae5b63d488ad06994ef12d81dc303b17472f3526", size = 95103981 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/66/82/564168a349148298aca281e342551404ef5521f33fba17b388ead0a84dc5/opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl", hash = "sha256:fc182f8f4cda51b45f01c64e4cbedfc2f00aff799debebc305d8d0210c43f251", size = 54835524 },
    { url = "https://files.pythonhosted.org/packages/64/4a/016cda9ad7cf18c58ba074628a4eaae8aa55f3fd06a266398cef8831a5b9/opencv_python-4.10.0.84-cp37-abi3-macosx_12_0_x86_64.whl", hash = "sha256:71e575744f1d23f79741450254660442785f45a0797212852ee5199ef12eed98", size = 56475426 },
    { url = "https://files.pythonhosted.org/packages/81/e4/7a987ebecfe5ceaf32db413b67ff18eb3092c598408862fff4d7cc3fd19b/opencv_python-4.10.0.84-cp37-abi3-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:09a332b50488e2dda866a6c5573ee192fe3583239fb26ff2f7f9ceb0bc119ea6", size = 41746971 },
    { url = "https://files.pythonhosted.org/packages/3f/a4/d2537f47fd7fcfba966bd806e3ec18e7ee1681056d4b0a9c8d983983e4d5/opencv_python-4.10.0.84-cp37-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:9ace140fc6d647fbe1c692bcb2abce768973491222c067c131d80957c595b71f", size = 62548253 },
    { url = "https://files.pythonhosted.org/packages/1e/39/bbf57e7b9dab623e8773f6ff36385456b7ae7fa9357a5e53db732c347eac/opencv_python-4.10.0.84-cp37-abi3-win32.whl", hash = "sha256:2db02bb7e50b703f0a2d50c50ced72e95c574e1e5a0bb35a8a86d0b35c98c236", size = 28737688 },
    { url = "https://files.pythonhosted.org/packages/ec/6c/fab8113424af5049f85717e8e527ca3773299a3c6b02506e66436e19874f/opencv_python-4.10.0.84-cp37-abi3-win_amd64.whl", hash = "sha256:32dbbd94c26f611dc5cc6979e6b7aa1f55a64d6b463cc1dcd3c95505a63e48fe", size = 38842521 },
]

[[package]]
name = "packaging"
version = "24.1"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/51/65/50db4dda066951078f0a96cf12f4b9ada6e4b811516bf0262c0f4f7064d4/packaging-24.1.tar.gz", hash = "sha256:026ed72c8ed3fcce5bf8950572258698927fd1dbda10a5e981cdf0ac37f4f002", size = 148788 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl", hash = "sha256:5b8f2217dbdbd2f7f384c41c628544e6d52f2d0f53c6d0c3ea61aa5d1d7ff124", size = 53985 },
]

[[package]]
name = "pillow"
version = "10.4.0"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/cd/74/ad3d526f3bf7b6d3f408b73fde271ec69dfac8b81341a318ce825f2b3812/pillow-10.4.0.tar.gz", hash = "sha256:166c1cd4d24309b30d61f79f4a9114b7b2313d7450912277855ff5dfd7cd4a06", size = 46555059 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/05/cb/0353013dc30c02a8be34eb91d25e4e4cf594b59e5a55ea1128fde1e5f8ea/pillow-10.4.0-cp312-cp312-macosx_10_10_x86_64.whl", hash = "sha256:673655af3eadf4df6b5457033f086e90299fdd7a47983a13827acf7459c15d94", size = 3509350 },
    { url = "https://files.pythonhosted.org/packages/e7/cf/5c558a0f247e0bf9cec92bff9b46ae6474dd736f6d906315e60e4075f737/pillow-10.4.0-cp312-cp312-macosx_11_0_arm64.whl", hash = "sha256:866b6942a92f56300012f5fbac71f2d610312ee65e22f1aa2609e491284e5597", size = 3374980 },
    { url = "https://files.pythonhosted.org/packages/84/48/6e394b86369a4eb68b8a1382c78dc092245af517385c086c5094e3b34428/pillow-10.4.0-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:29dbdc4207642ea6aad70fbde1a9338753d33fb23ed6956e706936706f52dd80", size = 4343799 },
    { url = "https://files.pythonhosted.org/packages/3b/f3/a8c6c11fa84b59b9df0cd5694492da8c039a24cd159f0f6918690105c3be/pillow-10.4.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:bf2342ac639c4cf38799a44950bbc2dfcb685f052b9e262f446482afaf4bffca", size = 4459973 },
    { url = "https://files.pythonhosted.org/packages/7d/1b/c14b4197b80150fb64453585247e6fb2e1d93761fa0fa9cf63b102fde822/pillow-10.4.0-cp312-cp312-manylinux_2_28_aarch64.whl", hash = "sha256:f5b92f4d70791b4a67157321c4e8225d60b119c5cc9aee8ecf153aace4aad4ef", size = 4370054 },
    { url = "https://files.pythonhosted.org/packages/55/77/40daddf677897a923d5d33329acd52a2144d54a9644f2a5422c028c6bf2d/pillow-10.4.0-cp312-cp312-manylinux_2_28_x86_64.whl", hash = "sha256:86dcb5a1eb778d8b25659d5e4341269e8590ad6b4e8b44d9f4b07f8d136c414a", size = 4539484 },
    { url = "https://files.pythonhosted.org/packages/40/54/90de3e4256b1207300fb2b1d7168dd912a2fb4b2401e439ba23c2b2cabde/pillow-10.4.0-cp312-cp312-musllinux_1_2_aarch64.whl", hash = "sha256:780c072c2e11c9b2c7ca37f9a2ee8ba66f44367ac3e5c7832afcfe5104fd6d1b", size = 4477375 },
    { url = "https://files.pythonhosted.org/packages/13/24/1bfba52f44193860918ff7c93d03d95e3f8748ca1de3ceaf11157a14cf16/pillow-10.4.0-cp312-cp312-musllinux_1_2_x86_64.whl", hash = "sha256:37fb69d905be665f68f28a8bba3c6d3223c8efe1edf14cc4cfa06c241f8c81d9", size = 4608773 },
    { url = "https://files.pythonhosted.org/packages/55/04/5e6de6e6120451ec0c24516c41dbaf80cce1b6451f96561235ef2429da2e/pillow-10.4.0-cp312-cp312-win32.whl", hash = "sha256:7dfecdbad5c301d7b5bde160150b4db4c659cee2b69589705b6f8a0c509d9f42", size = 2235690 },
    { url = "https://files.pythonhosted.org/packages/74/0a/d4ce3c44bca8635bd29a2eab5aa181b654a734a29b263ca8efe013beea98/pillow-10.4.0-cp312-cp312-win_amd64.whl", hash = "sha256:1d846aea995ad352d4bdcc847535bd56e0fd88d36829d2c90be880ef1ee4668a", size = 2554951 },
    { url = "https://files.pythonhosted.org/packages/b5/ca/184349ee40f2e92439be9b3502ae6cfc43ac4b50bc4fc6b3de7957563894/pillow-10.4.0-cp312-cp312-win_arm64.whl", hash = "sha256:e553cad5179a66ba15bb18b353a19020e73a7921296a7979c4a2b7f6a5cd57f9", size = 2243427 },
    { url = "https://files.pythonhosted.org/packages/c3/00/706cebe7c2c12a6318aabe5d354836f54adff7156fd9e1bd6c89f4ba0e98/pillow-10.4.0-cp313-cp313-macosx_10_13_x86_64.whl", hash = "sha256:8bc1a764ed8c957a2e9cacf97c8b2b053b70307cf2996aafd70e91a082e70df3", size = 3525685 },
    { url = "https://files.pythonhosted.org/packages/cf/76/f658cbfa49405e5ecbfb9ba42d07074ad9792031267e782d409fd8fe7c69/pillow-10.4.0-cp313-cp313-macosx_11_0_arm64.whl", hash = "sha256:6209bb41dc692ddfee4942517c19ee81b86c864b626dbfca272ec0f7cff5d9fb", size = 3374883 },
    { url = "https://files.pythonhosted.org/packages/46/2b/99c28c4379a85e65378211971c0b430d9c7234b1ec4d59b2668f6299e011/pillow-10.4.0-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:bee197b30783295d2eb680b311af15a20a8b24024a19c3a26431ff83eb8d1f70", size = 4339837 },
    { url = "https://files.pythonhosted.org/packages/f1/74/b1ec314f624c0c43711fdf0d8076f82d9d802afd58f1d62c2a86878e8615/pillow-10.4.0-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:1ef61f5dd14c300786318482456481463b9d6b91ebe5ef12f405afbba77ed0be", size = 4455562 },
    { url = "https://files.pythonhosted.org/packages/4a/2a/4b04157cb7b9c74372fa867096a1607e6fedad93a44deeff553ccd307868/pillow-10.4.0-cp313-cp313-manylinux_2_28_aarch64.whl", hash = "sha256:297e388da6e248c98bc4a02e018966af0c5f92dfacf5a5ca22fa01cb3179bca0", size = 4366761 },
    { url = "https://files.pythonhosted.org/packages/ac/7b/8f1d815c1a6a268fe90481232c98dd0e5fa8c75e341a75f060037bd5ceae/pillow-10.4.0-cp313-cp313-manylinux_2_28_x86_64.whl", hash = "sha256:e4db64794ccdf6cb83a59d73405f63adbe2a1887012e308828596100a0b2f6cc", size = 4536767 },
    { url = "https://files.pythonhosted.org/packages/e5/77/05fa64d1f45d12c22c314e7b97398ffb28ef2813a485465017b7978b3ce7/pillow-10.4.0-cp313-cp313-musllinux_1_2_aarch64.whl", hash = "sha256:bd2880a07482090a3bcb01f4265f1936a903d70bc740bfcb1fd4e8a2ffe5cf5a", size = 4477989 },
    { url = "https://files.pythonhosted.org/packages/12/63/b0397cfc2caae05c3fb2f4ed1b4fc4fc878f0243510a7a6034ca59726494/pillow-10.4.0-cp313-cp313-musllinux_1_2_x86_64.whl", hash = "sha256:4b35b21b819ac1dbd1233317adeecd63495f6babf21b7b2512d244ff6c6ce309", size = 4610255 },
    { url = "https://files.pythonhosted.org/packages/7b/f9/cfaa5082ca9bc4a6de66ffe1c12c2d90bf09c309a5f52b27759a596900e7/pillow-10.4.0-cp313-cp313-win32.whl", hash = "sha256:551d3fd6e9dc15e4c1eb6fc4ba2b39c0c7933fa113b220057a34f4bb3268a060", size = 2235603 },
    { url = "https://files.pythonhosted.org/packages/01/6a/30ff0eef6e0c0e71e55ded56a38d4859bf9d3634a94a88743897b5f96936/pillow-10.4.0-cp313-cp313-win_amd64.whl", hash = "sha256:030abdbe43ee02e0de642aee345efa443740aa4d828bfe8e2eb11922ea6a21ea", size = 2554972 },
    { url = "https://files.pythonhosted.org/packages/48/2c/2e0a52890f269435eee38b21c8218e102c621fe8d8df8b9dd06fabf879ba/pillow-10.4.0-cp313-cp313-win_arm64.whl", hash = "sha256:5b001114dd152cfd6b23befeb28d7aee43553e2402c9f159807bf55f33af8a8d", size = 2243375 },
]

[[package]]
name = "pluggy"
version = "1.5.0"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/96/2d/02d4312c973c6050a18b314a5ad0b3210edb65a906f868e31c111dede4a6/pluggy-1.5.0.tar.gz", hash = "sha256:2cffa88e94fdc978c4c574f15f9e59b7f4201d439195c3715ca9e2486f1d0cf1", size = 67955 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl", hash = "sha256:44e1ad92c8ca002de6377e165f3e0f1be63266ab4d554740532335b9d75ea669", size = 20556 },
]

[[package]]
name = "pyautogui"
version = "0.9.54"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "mouseinfo", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "pygetwindow", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "pymsgbox", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "pyobjc-core", marker = "platform_system == 'Darwin'" },
    { name = "pyobjc-framework-quartz", marker = "platform_system == 'Darwin'" },
    { name = "pyscreeze", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "python3-xlib", marker = "(platform_system == 'Linux' and python_full_version >= '3.12') or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "pytweening", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
]
sdist = { url = "https://files.pythonhosted.org/packages/65/ff/cdae0a8c2118a0de74b6cf4cbcdcaf8fd25857e6c3f205ce4b1794b27814/PyAutoGUI-0.9.54.tar.gz", hash = "sha256:dd1d29e8fd118941cb193f74df57e5c6ff8e9253b99c7b04f39cfc69f3ae04b2", size = 61236 }

[[package]]
name = "pygetwindow"
version = "0.0.9"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "pyrect", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
]
sdist = { url = "https://files.pythonhosted.org/packages/e1/70/c7a4f46dbf06048c6d57d9489b8e0f9c4c3d36b7479f03c5ca97eaa2541d/PyGetWindow-0.0.9.tar.gz", hash = "sha256:17894355e7d2b305cd832d717708384017c1698a90ce24f6f7fbf0242dd0a688", size = 9699 }

[[package]]
name = "pymsgbox"
version = "1.0.9"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/7d/ff/4c6f31a4f08979f12a663f2aeb6c8b765d3bd592e66eaaac445f547bb875/PyMsgBox-1.0.9.tar.gz", hash = "sha256:2194227de8bff7a3d6da541848705a155dcbb2a06ee120d9f280a1d7f51263ff", size = 18829 }

[[package]]
name = "pyobjc-core"
version = "10.3.1"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/b7/40/a38d78627bd882d86c447db5a195ff307001ae02c1892962c656f2fd6b83/pyobjc_core-10.3.1.tar.gz", hash = "sha256:b204a80ccc070f9ab3f8af423a3a25a6fd787e228508d00c4c30f8ac538ba720", size = 935027 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/cd/4d/d5d552b209fbca644cf9e0115a4cef8bc5f6726a44303eb7ae8d8a920a9e/pyobjc_core-10.3.1-cp312-cp312-macosx_10_9_universal2.whl", hash = "sha256:6ff5823d13d0a534cdc17fa4ad47cf5bee4846ce0fd27fc40012e12b46db571b", size = 825968 },
    { url = "https://files.pythonhosted.org/packages/2e/8b/341571ac5d625968083cbd718e1af7eac54197ed3d404dfff9467c3a8c88/pyobjc_core-10.3.1-cp313-cp313-macosx_10_13_universal2.whl", hash = "sha256:2581e8e68885bcb0e11ec619e81ef28e08ee3fac4de20d8cc83bc5af5bcf4a90", size = 827410 },
]

[[package]]
name = "pyobjc-framework-cocoa"
version = "10.3.1"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "pyobjc-core", marker = "platform_system == 'Darwin'" },
]
sdist = { url = "https://files.pythonhosted.org/packages/a7/6c/b62e31e6e00f24e70b62f680e35a0d663ba14ff7601ae591b5d20e251161/pyobjc_framework_cocoa-10.3.1.tar.gz", hash = "sha256:1cf20714daaa986b488fb62d69713049f635c9d41a60c8da97d835710445281a", size = 4941542 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/29/73/9a913537d6d63758243f76a3d3acbae8eb77705c278eceaf37198e58dcf5/pyobjc_framework_Cocoa-10.3.1-cp312-cp312-macosx_10_9_universal2.whl", hash = "sha256:11b4e0bad4bbb44a4edda128612f03cdeab38644bbf174de0c13129715497296", size = 396183 },
    { url = "https://files.pythonhosted.org/packages/93/1f/b203c35ac17ff50b101433783988b527c1b7d7386c175c9aec1c89da5426/pyobjc_framework_Cocoa-10.3.1-cp313-cp313-macosx_10_13_universal2.whl", hash = "sha256:de5e62e5ccf2871a94acf3bf79646b20ea893cc9db78afa8d1fe1b0d0f7cbdb0", size = 395004 },
]

[[package]]
name = "pyobjc-framework-quartz"
version = "10.3.1"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "pyobjc-core", marker = "platform_system == 'Darwin'" },
    { name = "pyobjc-framework-cocoa", marker = "platform_system == 'Darwin'" },
]
sdist = { url = "https://files.pythonhosted.org/packages/f7/a2/f488d801197b9b4d28d0b8d85947f9e2c8a6e89c5e6d4a828fc7cccfb57a/pyobjc_framework_quartz-10.3.1.tar.gz", hash = "sha256:b6d7e346d735c9a7f147cd78e6da79eeae416a0b7d3874644c83a23786c6f886", size = 3775947 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/0f/08/215f38dbebfca74f49276a9471531f360b4fb7888106f78953909919ca53/pyobjc_framework_Quartz-10.3.1-cp312-cp312-macosx_10_9_universal2.whl", hash = "sha256:ca35f92486869a41847a1703bb176aab8a53dbfd8e678d1f4d68d8e6e1581c71", size = 227195 },
    { url = "https://files.pythonhosted.org/packages/ce/7a/78b512061af37a4466607143a9876192f04c5810b16e4cb097fbbfa02dc5/pyobjc_framework_Quartz-10.3.1-cp313-cp313-macosx_10_13_universal2.whl", hash = "sha256:00a0933267e3a46ea4afcc35d117b2efb920f06de797fa66279c52e7057e3590", size = 226586 },
]

[[package]]
name = "pyperclip"
version = "1.9.0"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/30/23/2f0a3efc4d6a32f3b63cdff36cd398d9701d26cda58e3ab97ac79fb5e60d/pyperclip-1.9.0.tar.gz", hash = "sha256:b7de0142ddc81bfc5c7507eea19da920b92252b548b96186caf94a5e2527d310", size = 20961 }

[[package]]
name = "pyrect"
version = "0.2.0"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/cb/04/2ba023d5f771b645f7be0c281cdacdcd939fe13d1deb331fc5ed1a6b3a98/PyRect-0.2.0.tar.gz", hash = "sha256:f65155f6df9b929b67caffbd57c0947c5ae5449d3b580d178074bffb47a09b78", size = 17219 }

[[package]]
name = "pyscreeze"
version = "1.0.1"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/ee/f0/cb456ac4f1a73723d5b866933b7986f02bacea27516629c00f8e7da94c2d/pyscreeze-1.0.1.tar.gz", hash = "sha256:cf1662710f1b46aa5ff229ee23f367da9e20af4a78e6e365bee973cad0ead4be", size = 27826 }

[[package]]
name = "pytest"
version = "8.3.3"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "colorama", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12' and sys_platform == 'win32') or (platform_system != 'Linux' and python_full_version >= '3.12' and sys_platform == 'win32') or (platform_system == 'Darwin' and sys_platform == 'win32') or (platform_machine == 'aarch64' and platform_system == 'Linux' and sys_platform == 'win32')" },
    { name = "iniconfig", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "packaging", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "pluggy", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
]
sdist = { url = "https://files.pythonhosted.org/packages/8b/6c/62bbd536103af674e227c41a8f3dcd022d591f6eed5facb5a0f31ee33bbc/pytest-8.3.3.tar.gz", hash = "sha256:70b98107bd648308a7952b06e6ca9a50bc660be218d53c257cc1fc94fda10181", size = 1442487 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/6b/77/7440a06a8ead44c7757a64362dd22df5760f9b12dc5f11b6188cd2fc27a0/pytest-8.3.3-py3-none-any.whl", hash = "sha256:a6853c7375b2663155079443d2e45de913a911a11d669df02a50814944db57b2", size = 342341 },
]

[[package]]
name = "pytest-cov"
version = "5.0.0"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "coverage", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "pytest", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
]
sdist = { url = "https://files.pythonhosted.org/packages/74/67/00efc8d11b630c56f15f4ad9c7f9223f1e5ec275aaae3fa9118c6a223ad2/pytest-cov-5.0.0.tar.gz", hash = "sha256:5837b58e9f6ebd335b0f8060eecce69b662415b16dc503883a02f45dfeb14857", size = 63042 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/78/3a/af5b4fa5961d9a1e6237b530eb87dd04aea6eb83da09d2a4073d81b54ccf/pytest_cov-5.0.0-py3-none-any.whl", hash = "sha256:4f0764a1219df53214206bf1feea4633c3b558a2925c8b59f144f682861ce652", size = 21990 },
]

[[package]]
name = "pytest-html"
version = "4.1.1"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "jinja2", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "pytest", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
    { name = "pytest-metadata", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
]
sdist = { url = "https://files.pythonhosted.org/packages/bb/ab/4862dcb5a8a514bd87747e06b8d55483c0c9e987e1b66972336946e49b49/pytest_html-4.1.1.tar.gz", hash = "sha256:70a01e8ae5800f4a074b56a4cb1025c8f4f9b038bba5fe31e3c98eb996686f07", size = 150773 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/c8/c7/c160021cbecd956cc1a6f79e5fe155f7868b2e5b848f1320dad0b3e3122f/pytest_html-4.1.1-py3-none-any.whl", hash = "sha256:c8152cea03bd4e9bee6d525573b67bbc6622967b72b9628dda0ea3e2a0b5dd71", size = 23491 },
]

[[package]]
name = "pytest-metadata"
version = "3.1.1"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "pytest", marker = "(platform_machine != 'aarch64' and python_full_version >= '3.12') or (platform_system != 'Linux' and python_full_version >= '3.12') or platform_system == 'Darwin' or (platform_machine == 'aarch64' and platform_system == 'Linux')" },
]
sdist = { url = "https://files.pythonhosted.org/packages/a6/85/8c969f8bec4e559f8f2b958a15229a35495f5b4ce499f6b865eac54b878d/pytest_metadata-3.1.1.tar.gz", hash = "sha256:d2a29b0355fbc03f168aa96d41ff88b1a3b44a3b02acbe491801c98a048017c8", size = 9952 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/3e/43/7e7b2ec865caa92f67b8f0e9231a798d102724ca4c0e1f414316be1c1ef2/pytest_metadata-3.1.1-py3-none-any.whl", hash = "sha256:c8e0844db684ee1c798cfa38908d20d67d0463ecb6137c72e91f418558dd5f4b", size = 11428 },
]

[[package]]
name = "python3-xlib"
version = "0.15"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/ef/c6/2c5999de3bb1533521f1101e8fe56fd9c266732f4d48011c7c69b29d12ae/python3-xlib-0.15.tar.gz", hash = "sha256:dc4245f3ae4aa5949c1d112ee4723901ade37a96721ba9645f2bfa56e5b383f8", size = 132828 }

[[package]]
name = "pytweening"
version = "1.2.0"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/79/0c/c16bc93ac2755bac0066a8ecbd2a2931a1735a6fffd99a2b9681c7e83e90/pytweening-1.2.0.tar.gz", hash = "sha256:243318b7736698066c5f362ec5c2b6434ecf4297c3c8e7caa8abfe6af4cac71b", size = 171241 }

[[package]]
name = "rubicon-objc"
version = "0.4.9"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/6d/5f/13512d73208a16c4938622da1c497b14932e2874c0b062d535405d9c6ffb/rubicon_objc-0.4.9.tar.gz", hash = "sha256:3d77a5b2d10cb1e49679aa90b7824b46f67b3fd636229aa4a1b902d24aec6a58", size = 172844 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/0b/e3/f3d12557a4eed94dc88c780e0eac87f04d232d1a4f1beffc3fb0e50dc330/rubicon_objc-0.4.9-py3-none-any.whl", hash = "sha256:c351b3800cf74c8c23f7d534f008fd5de46c63818de7a44de96daffdb3ed8b8c", size = 63022 },
]

[[package]]
name = "structlog"
version = "24.4.0"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/78/a3/e811a94ac3853826805253c906faa99219b79951c7d58605e89c79e65768/structlog-24.4.0.tar.gz", hash = "sha256:b27bfecede327a6d2da5fbc96bd859f114ecc398a6389d664f62085ee7ae6fc4", size = 1348634 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/bf/65/813fc133609ebcb1299be6a42e5aea99d6344afb35ccb43f67e7daaa3b92/structlog-24.4.0-py3-none-any.whl", hash = "sha256:597f61e80a91cc0749a9fd2a098ed76715a1c8a01f73e336b746504d1aad7610", size = 67180 },
]

[[package]]
name = "timeout-decorator"
version = "0.5.0"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/80/f8/0802dd14c58b5d3d72bb9caa4315535f58787a1dc50b81bbbcaaa15451be/timeout-decorator-0.5.0.tar.gz", hash = "sha256:6a2f2f58db1c5b24a2cc79de6345760377ad8bdc13813f5265f6c3e63d16b3d7", size = 4754 }
```

---

_Comment by @milkpirate on 2024-10-15 18:28_

Could it be, that it tries to compile something, but does not find a compiler?

---

_Comment by @zanieb on 2024-10-15 18:35_

I can't really be sure. It's pretty unlikely this is a problem with uv. Does `pip install --use-pep517 <package>` work (i.e., without uv)?

---

_Comment by @milkpirate on 2024-10-15 19:46_

`pip`'s pipin'. Succeeds without any issues:
```bash
$ uv export > req.txt
Using CPython 3.13.0 interpreter at: /usr/local/bin/python
Resolved 31 packages in 2ms
$ pip install --use-pep517 -r req.txt
Ignoring colorama: markers '(platform_system == "Darwin" and sys_platform == "win32") or (platform_machine == "aarch64" and platform_s
ystem == "Linux" and sys_platform == "win32") or (python_full_version >= "3.12" and sys_platform == "win32")' don't match your environ
ment
...
Installing collected packages: timeout-decorator, pytweening, python3-xlib, pyscreeze, pyrect, pyperclip, pymsgbox, docopt, structlog, pygetwindow, pluggy, pillow, packaging, numpy, mouseinfo, markupsafe, iniconfig, coverage, pytest, pyautogui, opencv-python, jinja2, pytest-metadata, pytest-cov, pytest-html
Successfully installed coverage-7.6.2 docopt-0.6.2 iniconfig-2.0.0 jinja2-3.1.4 markupsafe-3.0.1 mouseinfo-0.1.3 numpy-2.1.2 opencv-python-4.10.0.84 packaging-24.1 pillow-10.4.0 pluggy-1.5.0 pyautogui-0.9.54 pygetwindow-0.0.9 pymsgbox-1.0.9 pyperclip-1.9.0 pyrect-0.2.0 pyscreeze-1.0.1 pytest-8.3.3 pytest-cov-5.0.0 pytest-html-4.1.1 pytest-metadata-3.1.1 python3-xlib-0.15 pytweening-1.2.0 structlog-24.4.0 timeout-decorator-0.5.0
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager, possibly rendering your system unusable.It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv. Use the --root-user-action option if you know what you are doing and want to suppress this warning.
$
```

---

_Comment by @zanieb on 2024-10-15 19:55_

Is there a reproducible example of your issue I could look at, e.g., on a GitHub Actions runner?

Does pip build the packages from source too?

---

_Comment by @milkpirate on 2024-10-15 20:55_

See above, I pasted the `pyproject.toml` and `uv.lock`. I use the same fresh docker image `ghcr.io/astral-sh/uv:python3.13-bookworm-slim` for either (`pip` / `uv` install). No, pip does not build stuff from source. 

---

_Comment by @zanieb on 2024-10-16 04:32_

I can't reproduce this, e.g. I ran that Docker image, added your project definition, then ran `uv pip install --system --verbose --no-cache-dir .` and it worked fine. I'm on an aarch64 machine though, hence my request for a reproduction somewhere concrete like GitHub Actions.

---

_Comment by @zanieb on 2024-10-16 04:35_

I also cannot reproduce this in a container with `docker run --platform linux/amd64 -it ghcr.io/astral-sh/uv:python3.13-bookworm-slim /bin/bash`

---

_Label `needs-mre` added by @charliermarsh on 2024-10-16 18:46_

---

_Comment by @milkpirate on 2024-10-17 16:08_

Ok, now I cant even reproduce myself anymore. I think the problem is solved. Will open this issue again if I have new problems. Sorry for bothering...

---

_Closed by @milkpirate on 2024-10-17 16:08_

---

_Comment by @hughlv on 2025-04-27 15:44_

I'm having similar issue.

This command is stuck:
```bash
uv pip install -e .
```

This worked:
```bash
uv pip install --system --verbose --no-cache-dir -e .
```

This command is stuck with more info:
```bash
❯ uv pip install --verbose --no-cache-dir -e .
DEBUG uv 0.6.8 (c1ef48276 2025-03-18)
DEBUG Marking explicit source tree for reinstall: `/Users/hugh/tc/docxy/server`
DEBUG Searching for default Python interpreter in virtual environments
```

It's quite clear that the searching for interpreter in virtual environment will hang up. It will succeed if installing to system environment.

---

_Comment by @charliermarsh on 2025-04-27 15:47_

Unfortunately we'll need a clear reproduction in order to help you out here.

---
