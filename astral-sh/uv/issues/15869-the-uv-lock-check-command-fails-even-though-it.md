---
number: 15869
title: The uv lock --check command fails even though it should pass.
type: issue
state: closed
author: yshiftanlightricks
labels:
  - bug
  - great writeup
assignees: []
created_at: 2025-09-15T08:07:55Z
updated_at: 2025-10-01T15:03:44Z
url: https://github.com/astral-sh/uv/issues/15869
synced_at: 2026-01-10T01:26:01Z
---

# The uv lock --check command fails even though it should pass.

---

_Issue opened by @yshiftanlightricks on 2025-09-15 08:07_

### Summary

The problem is that after running uv lock, the --check command still fails.

the pyproject.toml content is:

```
[project]
name = "uv_bug"
version = "0.1.0"
requires-python = ">=3.10,<3.11"

[tool.uv]
conflicts = [
  [
    { group = "numpy" },
    { group = "numpy_old" },
  ]
]


[dependency-groups]
numpy = [
  "numpy==2.0.0",
]
numpy_old = [
  "numpy<2.0.0",
]
include_numpy = [
  {include-group = "numpy"},
]
```

terminal output:
```
➜  ~/repos/txt2img git:(poetry_to_uv) ✗ uv lock && uv lock --check --verbose
Resolved 3 packages in 6ms
DEBUG uv 0.8.17 (10960bc13 2025-09-10)
DEBUG Found workspace root: `/Users/yshiftan/repos/txt2img`
DEBUG Adding root workspace member: `/Users/yshiftan/repos/txt2img`
DEBUG No Python version file found in workspace: /Users/yshiftan/repos/txt2img
DEBUG Using Python request `==3.10.*` from `requires-python` metadata
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python ==3.10.*`
DEBUG Using request timeout of 30s
DEBUG Resolving despite existing lockfile due to change in conflicting groups: `Conflicts([ConflictSet { set: {ConflictItem { package: PackageName("uv-bug"), kind: Group(GroupName("numpy")) }, ConflictItem { package: PackageName("uv-bug"), kind: Group(GroupName("numpy-old")) }}, is_inferred_conflict: false }, ConflictSet { set: {ConflictItem { package: PackageName("uv-bug"), kind: Group(GroupName("include-numpy")) }, ConflictItem { package: PackageName("uv-bug"), kind: Group(GroupName("numpy-old")) }}, is_inferred_conflict: true }])` vs. `Conflicts([ConflictSet { set: {ConflictItem { package: PackageName("uv-bug"), kind: Group(GroupName("numpy")) }, ConflictItem { package: PackageName("uv-bug"), kind: Group(GroupName("numpy-old")) }}, is_inferred_conflict: false }, ConflictSet { set: {ConflictItem { package: PackageName("uv-bug"), kind: Group(GroupName("include-numpy")) }, ConflictItem { package: PackageName("uv-bug"), kind: Group(GroupName("numpy-old")) }}, is_inferred_conflict: false }])`
DEBUG Found static `pyproject.toml` for: uv-bug @ file:///Users/yshiftan/repos/txt2img
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.10.18
DEBUG Solving with target Python version: ==3.10.*
DEBUG Pre-fork all marker environments took 0.000s
DEBUG Splitting resolution on root==0a0.dev0 over uv-bug into 5 resolutions with separate markers
DEBUG Adding direct dependency: uv-bug*
DEBUG Adding direct dependency: uv-bug*
DEBUG Adding direct dependency: uv-bug*
DEBUG Adding direct dependency: uv-bug*
DEBUG Adding direct dependency: uv-bug:include-numpy*
DEBUG Adding direct dependency: uv-bug*
DEBUG Adding direct dependency: uv-bug*
DEBUG Adding direct dependency: uv-bug:numpy*
DEBUG Adding direct dependency: uv-bug*
DEBUG Adding direct dependency: uv-bug*
DEBUG Adding direct dependency: uv-bug:include-numpy*
DEBUG Adding direct dependency: uv-bug:numpy*
DEBUG Adding direct dependency: uv-bug*
DEBUG Adding direct dependency: uv-bug*
DEBUG Adding direct dependency: uv-bug:numpy-old*
DEBUG Solving split (included: uv-bug[group:numpy-old]; excluded: uv-bug[group:include-numpy], uv-bug[group:numpy]) (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: EqualStar, version: "3.10" }]), range: RequiresPythonRange(LowerBound(Included("3.10")), UpperBound(Excluded("3.11"))) })
DEBUG Searching for a compatible version of uv-bug @ file:///Users/yshiftan/repos/txt2img (*)
DEBUG Searching for a compatible version of uv-bug @ file:///Users/yshiftan/repos/txt2img (*)
DEBUG Adding direct dependency: uv-bug:numpy-old==0.1.0
DEBUG Searching for a compatible version of uv-bug @ file:///Users/yshiftan/repos/txt2img (==0.1.0)
DEBUG Adding direct dependency: numpy<2.0.0
DEBUG Found fresh response for: https://pypi.org/simple/numpy/
DEBUG Searching for a compatible version of numpy (<2.0.0)
DEBUG Selecting: numpy==1.26.4 [preference] (numpy-1.26.4-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a7/94/ace0fdea5241a27d13543ee117cbc65868e82213fb31a8eb7fe9ff23f313/numpy-1.26.4-cp310-cp310-macosx_10_9_x86_64.whl.metadata
DEBUG Tried 2 versions: numpy 1, uv-bug 1
DEBUG all marker environments resolution took 0.001s
DEBUG Solving split (included: uv-bug[group:include-numpy], uv-bug[group:numpy]; excluded: uv-bug[group:numpy-old]) (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: EqualStar, version: "3.10" }]), range: RequiresPythonRange(LowerBound(Included("3.10")), UpperBound(Excluded("3.11"))) })
DEBUG Searching for a compatible version of uv-bug @ file:///Users/yshiftan/repos/txt2img (*)
DEBUG Searching for a compatible version of uv-bug @ file:///Users/yshiftan/repos/txt2img (*)
DEBUG Adding direct dependency: uv-bug:include-numpy==0.1.0
DEBUG Searching for a compatible version of uv-bug @ file:///Users/yshiftan/repos/txt2img (*)
DEBUG Adding direct dependency: uv-bug:numpy==0.1.0
DEBUG Searching for a compatible version of uv-bug @ file:///Users/yshiftan/repos/txt2img (==0.1.0)
DEBUG Adding direct dependency: numpy>=2.0.0, <2.0.0+
DEBUG Searching for a compatible version of uv-bug @ file:///Users/yshiftan/repos/txt2img (==0.1.0)
DEBUG Adding direct dependency: numpy>=2.0.0, <2.0.0+
DEBUG Searching for a compatible version of numpy (>=2.0.0, <2.0.0+)
DEBUG Selecting: numpy==2.0.0 [preference] (numpy-2.0.0-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3a/83/24dafa898f172e198a1c164eb01675bbcbf5895ac8f9b1f8078ea5c2fdb5/numpy-2.0.0-cp310-cp310-macosx_10_9_x86_64.whl.metadata
DEBUG Tried 2 versions: numpy 1, uv-bug 1
DEBUG all marker environments resolution took 0.000s
DEBUG Solving split (included: uv-bug[group:numpy]; excluded: uv-bug[group:include-numpy], uv-bug[group:numpy-old]) (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: EqualStar, version: "3.10" }]), range: RequiresPythonRange(LowerBound(Included("3.10")), UpperBound(Excluded("3.11"))) })
DEBUG Searching for a compatible version of uv-bug @ file:///Users/yshiftan/repos/txt2img (*)
DEBUG Searching for a compatible version of uv-bug @ file:///Users/yshiftan/repos/txt2img (*)
DEBUG Adding direct dependency: uv-bug:numpy==0.1.0
DEBUG Searching for a compatible version of uv-bug @ file:///Users/yshiftan/repos/txt2img (==0.1.0)
DEBUG Adding direct dependency: numpy>=2.0.0, <2.0.0+
DEBUG Searching for a compatible version of numpy (>=2.0.0, <2.0.0+)
DEBUG Selecting: numpy==2.0.0 [preference] (numpy-2.0.0-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Tried 2 versions: numpy 1, uv-bug 1
DEBUG all marker environments resolution took 0.000s
DEBUG Solving split (included: uv-bug[group:include-numpy]; excluded: uv-bug[group:numpy-old], uv-bug[group:numpy]) (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: EqualStar, version: "3.10" }]), range: RequiresPythonRange(LowerBound(Included("3.10")), UpperBound(Excluded("3.11"))) })
DEBUG Searching for a compatible version of uv-bug @ file:///Users/yshiftan/repos/txt2img (*)
DEBUG Searching for a compatible version of uv-bug @ file:///Users/yshiftan/repos/txt2img (*)
DEBUG Adding direct dependency: uv-bug:include-numpy==0.1.0
DEBUG Searching for a compatible version of uv-bug @ file:///Users/yshiftan/repos/txt2img (==0.1.0)
DEBUG Adding direct dependency: numpy>=2.0.0, <2.0.0+
DEBUG Searching for a compatible version of numpy (>=2.0.0, <2.0.0+)
DEBUG Selecting: numpy==2.0.0 [preference] (numpy-2.0.0-cp310-cp310-macosx_10_9_x86_64.whl)
DEBUG Tried 2 versions: numpy 1, uv-bug 1
DEBUG all marker environments resolution took 0.000s
DEBUG Solving split (excluded: uv-bug[group:include-numpy], uv-bug[group:numpy-old], uv-bug[group:numpy]) (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: EqualStar, version: "3.10" }]), range: RequiresPythonRange(LowerBound(Included("3.10")), UpperBound(Excluded("3.11"))) })
DEBUG Searching for a compatible version of uv-bug @ file:///Users/yshiftan/repos/txt2img (*)
DEBUG Tried 1 versions: uv-bug 1
DEBUG all marker environments resolution took 0.000s
INFO Solved your requirements for 5 environments
DEBUG Distinct solution for split (included: uv-bug[group:numpy-old]; excluded: uv-bug[group:include-numpy], uv-bug[group:numpy]) with 2 package(s)
DEBUG Distinct solution for split (included: uv-bug[group:include-numpy], uv-bug[group:numpy]; excluded: uv-bug[group:numpy-old]) with 2 package(s)
DEBUG Distinct solution for split (included: uv-bug[group:numpy]; excluded: uv-bug[group:include-numpy], uv-bug[group:numpy-old]) with 2 package(s)
DEBUG Distinct solution for split (included: uv-bug[group:include-numpy]; excluded: uv-bug[group:numpy-old], uv-bug[group:numpy]) with 2 package(s)
DEBUG Distinct solution for split (excluded: uv-bug[group:include-numpy], uv-bug[group:numpy-old], uv-bug[group:numpy]) with 1 package(s)
Resolved 3 packages in 5ms
The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
```
### Platform

macOS 15.6.1

### Version

uv 0.8.17 (10960bc13 2025-09-10)

### Python version

Python 3.10.18

---

_Label `bug` added by @yshiftanlightricks on 2025-09-15 08:07_

---

_Comment by @konstin on 2025-09-15 08:14_

Thank you for the minimal example!

---

_Label `great writeup` added by @konstin on 2025-09-15 08:14_

---

_Comment by @konstin on 2025-09-15 08:23_

CC @burntsushi @charliermarsh there's a difference in `is_inferred_conflict` with `include-group`:

```
Conflicts([ConflictSet { set: {ConflictItem { package: PackageName("uv-bug"), kind: Group(GroupName("numpy")) }, ConflictItem { package: PackageName("uv-bug"), kind: Group(GroupName("numpy-old")) }}, is_inferred_conflict: false }, ConflictSet { set: {ConflictItem { package: PackageName("uv-bug"), kind: Group(GroupName("include-numpy")) }, ConflictItem { package: PackageName("uv-bug"), kind: Group(GroupName("numpy-old")) }}, is_inferred_conflict: true }])
```
```
Conflicts([ConflictSet { set: {ConflictItem { package: PackageName("uv-bug"), kind: Group(GroupName("numpy")) }, ConflictItem { package: PackageName("uv-bug"), kind: Group(GroupName("numpy-old")) }}, is_inferred_conflict: false }, ConflictSet { set: {ConflictItem { package: PackageName("uv-bug"), kind: Group(GroupName("include-numpy")) }, ConflictItem { package: PackageName("uv-bug"), kind: Group(GroupName("numpy-old")) }}, is_inferred_conflict: false }])
```


---

_Comment by @zanieb on 2025-09-16 02:12_

Possibly related to https://github.com/astral-sh/uv/pull/12005

---

_Comment by @zanieb on 2025-09-16 02:15_

Presumably the problem is that in the lockfile we store the resolved group

```
version = 1
revision = 3
requires-python = "==3.10.*"
conflicts = [[
    { package = "uv-bug", group = "numpy" },
    { package = "uv-bug", group = "numpy-old" },
], [
    { package = "uv-bug", group = "include-numpy" },
    { package = "uv-bug", group = "numpy-old" },
]]

[[package]]
name = "numpy"
version = "1.26.4"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/65/6e/09db70a523a96d25e115e71cc56a6f9031e7b8cd166c1ac8438307c14058/numpy-1.26.4.tar.gz", hash = "sha256:2a02aba9ed12e4ac4eb3ea9421c420301a0c6460d9830d74a9df87efa4912010", size = 15786129, upload-time = "2024-02-06T00:26:44.495Z" }
wheels = [
    { url = "https://files.pythonhosted.org/packages/a7/94/ace0fdea5241a27d13543ee117cbc65868e82213fb31a8eb7fe9ff23f313/numpy-1.26.4-cp310-cp310-macosx_10_9_x86_64.whl", hash = "sha256:9ff0f4f29c51e2803569d7a51c2304de5554655a60c5d776e35b4a41413830d0", size = 20631468, upload-time = "2024-02-05T23:48:01.194Z" },
    { url = "https://files.pythonhosted.org/packages/20/f7/b24208eba89f9d1b58c1668bc6c8c4fd472b20c45573cb767f59d49fb0f6/numpy-1.26.4-cp310-cp310-macosx_11_0_arm64.whl", hash = "sha256:2e4ee3380d6de9c9ec04745830fd9e2eccb3e6cf790d39d7b98ffd19b0dd754a", size = 13966411, upload-time = "2024-02-05T23:48:29.038Z" },
    { url = "https://files.pythonhosted.org/packages/fc/a5/4beee6488160798683eed5bdb7eead455892c3b4e1f78d79d8d3f3b084ac/numpy-1.26.4-cp310-cp310-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:d209d8969599b27ad20994c8e41936ee0964e6da07478d6c35016bc386b66ad4", size = 14219016, upload-time = "2024-02-05T23:48:54.098Z" },
    { url = "https://files.pythonhosted.org/packages/4b/d7/ecf66c1cd12dc28b4040b15ab4d17b773b87fa9d29ca16125de01adb36cd/numpy-1.26.4-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:ffa75af20b44f8dba823498024771d5ac50620e6915abac414251bd971b4529f", size = 18240889, upload-time = "2024-02-05T23:49:25.361Z" },
    { url = "https://files.pythonhosted.org/packages/24/03/6f229fe3187546435c4f6f89f6d26c129d4f5bed40552899fcf1f0bf9e50/numpy-1.26.4-cp310-cp310-musllinux_1_1_aarch64.whl", hash = "sha256:62b8e4b1e28009ef2846b4c7852046736bab361f7aeadeb6a5b89ebec3c7055a", size = 13876746, upload-time = "2024-02-05T23:49:51.983Z" },
    { url = "https://files.pythonhosted.org/packages/39/fe/39ada9b094f01f5a35486577c848fe274e374bbf8d8f472e1423a0bbd26d/numpy-1.26.4-cp310-cp310-musllinux_1_1_x86_64.whl", hash = "sha256:a4abb4f9001ad2858e7ac189089c42178fcce737e4169dc61321660f1a96c7d2", size = 18078620, upload-time = "2024-02-05T23:50:22.515Z" },
    { url = "https://files.pythonhosted.org/packages/d5/ef/6ad11d51197aad206a9ad2286dc1aac6a378059e06e8cf22cd08ed4f20dc/numpy-1.26.4-cp310-cp310-win32.whl", hash = "sha256:bfe25acf8b437eb2a8b2d49d443800a5f18508cd811fea3181723922a8a82b07", size = 5972659, upload-time = "2024-02-05T23:50:35.834Z" },
    { url = "https://files.pythonhosted.org/packages/19/77/538f202862b9183f54108557bfda67e17603fc560c384559e769321c9d92/numpy-1.26.4-cp310-cp310-win_amd64.whl", hash = "sha256:b97fe8060236edf3662adfc2c633f56a08ae30560c56310562cb4f95500022d5", size = 15808905, upload-time = "2024-02-05T23:51:03.701Z" },
]

[[package]]
name = "numpy"
version = "2.0.0"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/05/35/fb1ada118002df3fe91b5c3b28bc0d90f879b881a5d8f68b1f9b79c44bfe/numpy-2.0.0.tar.gz", hash = "sha256:cf5d1c9e6837f8af9f92b6bd3e86d513cdc11f60fd62185cc49ec7d1aba34864", size = 18326228, upload-time = "2024-06-16T13:24:44.066Z" }
wheels = [
    { url = "https://files.pythonhosted.org/packages/3a/83/24dafa898f172e198a1c164eb01675bbcbf5895ac8f9b1f8078ea5c2fdb5/numpy-2.0.0-cp310-cp310-macosx_10_9_x86_64.whl", hash = "sha256:04494f6ec467ccb5369d1808570ae55f6ed9b5809d7f035059000a37b8d7e86f", size = 21214540, upload-time = "2024-06-16T13:06:46.93Z" },
    { url = "https://files.pythonhosted.org/packages/b6/8f/780b1719bee25794115b23dafd022aa4a835002077df58d4234ca6a23143/numpy-2.0.0-cp310-cp310-macosx_11_0_arm64.whl", hash = "sha256:2635dbd200c2d6faf2ef9a0d04f0ecc6b13b3cad54f7c67c61155138835515d2", size = 13307901, upload-time = "2024-06-16T13:07:10.426Z" },
    { url = "https://files.pythonhosted.org/packages/3b/61/e1e77694c4ed929c8edebde7d2ac30dbf3ed452c1988b633569d3d7ff271/numpy-2.0.0-cp310-cp310-macosx_14_0_arm64.whl", hash = "sha256:0a43f0974d501842866cc83471bdb0116ba0dffdbaac33ec05e6afed5b615238", size = 5238781, upload-time = "2024-06-16T13:07:20.486Z" },
    { url = "https://files.pythonhosted.org/packages/fc/1f/34b58ba54b5f202728083b5007d4b27dfcfd0edc616addadb0b35c7817d7/numpy-2.0.0-cp310-cp310-macosx_14_0_x86_64.whl", hash = "sha256:8d83bb187fb647643bd56e1ae43f273c7f4dbcdf94550d7938cfc32566756514", size = 6882511, upload-time = "2024-06-16T13:07:31.871Z" },
    { url = "https://files.pythonhosted.org/packages/e1/5f/e51e3ebdaad1bccffdf9ba4b979c8b2fe2bd376d10bf9e9b59e1c6972a1a/numpy-2.0.0-cp310-cp310-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:79e843d186c8fb1b102bef3e2bc35ef81160ffef3194646a7fdd6a73c6b97196", size = 13904765, upload-time = "2024-06-16T13:07:53.175Z" },
    { url = "https://files.pythonhosted.org/packages/d6/a8/6a2419c40c7b6f7cb4ef52c532c88e55490c4fa92885964757d507adddce/numpy-2.0.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:6d7696c615765091cc5093f76fd1fa069870304beaccfd58b5dcc69e55ef49c1", size = 19282097, upload-time = "2024-06-16T13:08:22.24Z" },
    { url = "https://files.pythonhosted.org/packages/52/d3/74989fffc21c74fba73eb05591cf3a56aaa135ee2427826217487028abd0/numpy-2.0.0-cp310-cp310-musllinux_1_1_x86_64.whl", hash = "sha256:b4c76e3d4c56f145d41b7b6751255feefae92edbc9a61e1758a98204200f30fc", size = 19699985, upload-time = "2024-06-16T13:08:51.42Z" },
    { url = "https://files.pythonhosted.org/packages/23/8a/a5cac659347f916cfaf2343eba577e98c83edd1ad6ada5586018961bf667/numpy-2.0.0-cp310-cp310-musllinux_1_2_aarch64.whl", hash = "sha256:acd3a644e4807e73b4e1867b769fbf1ce8c5d80e7caaef0d90dcdc640dfc9787", size = 14406309, upload-time = "2024-06-16T13:09:13.701Z" },
    { url = "https://files.pythonhosted.org/packages/8b/c4/858aadfd1f3f2f815c03be62556115f43796b805943755a9aef5b6b29b04/numpy-2.0.0-cp310-cp310-win32.whl", hash = "sha256:cee6cc0584f71adefe2c908856ccc98702baf95ff80092e4ca46061538a2ba98", size = 6358424, upload-time = "2024-06-16T13:09:25.522Z" },
    { url = "https://files.pythonhosted.org/packages/9c/de/7d17991e0683f84bcfefcf4e3f43da6b37155b9e6a0429942494f044a7ef/numpy-2.0.0-cp310-cp310-win_amd64.whl", hash = "sha256:ed08d2703b5972ec736451b818c2eb9da80d66c3e84aed1deeb0c345fefe461b", size = 16507217, upload-time = "2024-06-16T13:09:50.234Z" },
]

[[package]]
name = "uv-bug"
version = "0.1.0"
source = { virtual = "." }

[package.dev-dependencies]
include-numpy = [
    { name = "numpy", version = "2.0.0", source = { registry = "https://pypi.org/simple" } },
]
numpy = [
    { name = "numpy", version = "2.0.0", source = { registry = "https://pypi.org/simple" } },
]
numpy-old = [
    { name = "numpy", version = "1.26.4", source = { registry = "https://pypi.org/simple" } },
]

[package.metadata]

[package.metadata.requires-dev]
include-numpy = [{ name = "numpy", specifier = "==2.0.0" }]
numpy = [{ name = "numpy", specifier = "==2.0.0" }]
numpy-old = [{ name = "numpy", specifier = "<2.0.0" }]
```

---

_Referenced in [astral-sh/uv#15884](../../astral-sh/uv/pulls/15884.md) on 2025-09-16 02:30_

---

_Comment by @zanieb on 2025-09-16 02:38_

I have a patch, but it'll need discussion https://github.com/astral-sh/uv/pull/15884#discussion_r2350517900

---

_Referenced in [astral-sh/uv#15909](../../astral-sh/uv/pulls/15909.md) on 2025-09-17 15:50_

---

_Closed by @zanieb on 2025-10-01 15:03_

---
