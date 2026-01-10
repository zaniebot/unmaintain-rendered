```yaml
number: 3551
title: Clone individual files on windows ReFS
type: pull_request
state: merged
author: bschoenmaeckers
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: windows-reflink
created_at: 2024-05-13T15:52:53Z
updated_at: 2024-05-13T16:03:46Z
url: https://github.com/astral-sh/uv/pull/3551
synced_at: 2026-01-10T14:37:54Z
```

# Clone individual files on windows ReFS

---

_Pull request opened by @bschoenmaeckers on 2024-05-13 15:52_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

Windows does not support cloning whole directories so clone each file instead.

closes #3547 

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Ran ` uv pip install setuptools --link-mode=clone` manually 

<!-- How was it tested? -->


---

_@charliermarsh approved on 2024-05-13 15:53_

---

_Comment by @charliermarsh on 2024-05-13 15:53_

Were you able to test this locally?

---

_Label `bug` added by @charliermarsh on 2024-05-13 15:54_

---

_Label `windows` added by @charliermarsh on 2024-05-13 15:54_

---

_Comment by @bschoenmaeckers on 2024-05-13 16:02_

Yes testing locally succeeds now.

<details>
  <summary>log trace</summary>

```console
>uv pip install setuptools --link-mode=clone
INFO Found a virtualenv through VIRTUAL_ENV at: D:\repo\test-project\.venv
DEBUG Cached interpreter info for Python 3.12.1, skipping probing: .venv\Scripts\python.exe
DEBUG Using Python 3.12.1 environment at .venv\Scripts\python.exe
DEBUG Trying to lock if free: .venv\.lock
DEBUG At least one requirement is not satisfied: setuptools
DEBUG Using registry request timeout of 30s
⠋ Resolving dependencies...                                                                                                                                                                                                                                                                                         DEBUG Solving with target Python version 3.12.1
DEBUG Adding direct dependency: setuptools*
⠙ Resolving dependencies...                                                                                                                                                                                                                                                                                         INFO add_decision: root @ 0a0.dev0
TRACE Fetching metadata for setuptools from https://pypi.org/simple/setuptools/
TRACE cached request https://pypi.org/simple/setuptools/ is storable because its response has a 'public' cache-control directive
TRACE cached request https://pypi.org/simple/setuptools/ has a cached response that does not allow staleness
TRACE request https://pypi.org/simple/setuptools/ does not have a fresh cache because its age is 975 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.org/simple/setuptools/
DEBUG Sending revalidation request for: https://pypi.org/simple/setuptools/
TRACE Handling request for https://pypi.org/simple/setuptools/
TRACE Request for https://pypi.org/simple/setuptools/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/setuptools/
TRACE Attempting unauthenticated request for https://pypi.org/simple/setuptools/
DEBUG starting new connection: https://pypi.org/
DEBUG resolving host="pypi.org"
⠹ Resolving dependencies...                                                                                                                                                                                                                                                                                         DEBUG connecting to 151.101.192.223:443
DEBUG connected to 151.101.192.223:443
DEBUG No cached session for DnsName("pypi.org")
DEBUG Not resuming any session
DEBUG Using ciphersuite TLS13_AES_128_GCM_SHA256
DEBUG Not resuming
DEBUG TLS1.3 encrypted extensions: [ServerNameAck, Protocols([ProtocolName(687474702f312e31)])]
DEBUG ALPN protocol is Some(b"http/1.1")
DEBUG pooling idle connection for ("https", pypi.org)
TRACE not modified because old and new etag values ([34, 119, 109, 114, 47, 104, 57, 106, 103, 50, 82, 67, 68, 89, 100, 82, 65, 103, 66, 112, 107, 75, 81, 34]) match
DEBUG Found not-modified response for: https://pypi.org/simple/setuptools/
TRACE Received package metadata for: setuptools
TRACE selecting candidate for package setuptools with range Range { segments: [(Unbounded, Unbounded)] } with 557 remote versions
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "69.5.1" version
DEBUG Searching for a compatible version of setuptools (*)
TRACE selecting candidate for package setuptools with range Range { segments: [(Unbounded, Unbounded)] } with 557 remote versions
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "69.5.1" version
DEBUG Selecting: setuptools==69.5.1 (setuptools-69.5.1-py3-none-any.whl)
⠹ setuptools==69.5.1                                                                                                                                                                                                                                                                                                TRACE cached request https://files.pythonhosted.org/packages/f7/29/13965af254e3373bceae8fb9a0e6ea0d0e571171b80d6646932131d6439b/setuptools-69.5.1-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f7/29/13965af254e3373bceae8fb9a0e6ea0d0e571171b80d6646932131d6439b/setuptools-69.5.1-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: setuptools==69.5.1
INFO add_decision: setuptools @ 69.5.1
DEBUG Tried 2 versions: root 1, setuptools 1
Resolved 1 package in 315ms
DEBUG Requirement already cached: setuptools==69.5.1
DEBUG Unnecessary package: pip==24.0
░░░░░░░░░░░░░░░░░░░░ [0/1] Installing wheels...                                                                                                                                                                                                                                                                     DEBUG Extracting file name="setuptools"
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\distutils-precedence.pth to D:\repo\test-project\.venv\Lib\site-packages\distutils-precedence.pth
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\extern to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\extern
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\extern\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\extern\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\backports to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\backports
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\backports\tarfile.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\backports\tarfile.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\backports\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\backports\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\importlib_resources to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\importlib_resources
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\importlib_resources\abc.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\importlib_resources\abc.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\importlib_resources\py.typed to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\importlib_resources\py.typed
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\importlib_resources\readers.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\importlib_resources\readers.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\importlib_resources\simple.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\importlib_resources\simple.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\importlib_resources\_adapters.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\importlib_resources\_adapters.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\importlib_resources\_common.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\importlib_resources\_common.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\importlib_resources\_compat.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\importlib_resources\_compat.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\importlib_resources\_itertools.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\importlib_resources\_itertools.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\importlib_resources\_legacy.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\importlib_resources\_legacy.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\importlib_resources\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\importlib_resources\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\jaraco to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\jaraco
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\jaraco\context.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\jaraco\context.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\jaraco\functools to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\jaraco\functools
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\jaraco\functools\py.typed to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\jaraco\functools\py.typed
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\jaraco\functools\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\jaraco\functools\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\jaraco\functools\__init__.pyi to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\jaraco\functools\__init__.pyi
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\jaraco\text to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\jaraco\text
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\jaraco\text\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\jaraco\text\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\jaraco\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\jaraco\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\more_itertools to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\more_itertools
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\more_itertools\more.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\more_itertools\more.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\more_itertools\more.pyi to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\more_itertools\more.pyi
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\more_itertools\py.typed to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\more_itertools\py.typed
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\more_itertools\recipes.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\more_itertools\recipes.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\more_itertools\recipes.pyi to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\more_itertools\recipes.pyi
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\more_itertools\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\more_itertools\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\more_itertools\__init__.pyi to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\more_itertools\__init__.pyi
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\packaging to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\packaging
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\packaging\markers.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\packaging\markers.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\packaging\metadata.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\packaging\metadata.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\packaging\py.typed to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\packaging\py.typed
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\packaging\requirements.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\packaging\requirements.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\packaging\specifiers.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\packaging\specifiers.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\packaging\tags.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\packaging\tags.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\packaging\utils.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\packaging\utils.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\packaging\version.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\packaging\version.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\packaging\_elffile.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\packaging\_elffile.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\packaging\_manylinux.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\packaging\_manylinux.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\packaging\_musllinux.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\packaging\_musllinux.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\packaging\_parser.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\packaging\_parser.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\packaging\_structures.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\packaging\_structures.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\packaging\_tokenizer.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\packaging\_tokenizer.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\packaging\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\packaging\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\platformdirs to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\platformdirs
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\platformdirs\android.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\platformdirs\android.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\platformdirs\api.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\platformdirs\api.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\platformdirs\macos.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\platformdirs\macos.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\platformdirs\py.typed to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\platformdirs\py.typed
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\platformdirs\unix.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\platformdirs\unix.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\platformdirs\version.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\platformdirs\version.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\platformdirs\windows.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\platformdirs\windows.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\platformdirs\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\platformdirs\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\platformdirs\__main__.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\platformdirs\__main__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\typing_extensions.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\typing_extensions.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\zipp.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\zipp.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\_vendor\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\_vendor\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools to D:\repo\test-project\.venv\Lib\site-packages\setuptools
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\archive_util.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\archive_util.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\build_meta.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\build_meta.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\cli-32.exe to D:\repo\test-project\.venv\Lib\site-packages\setuptools\cli-32.exe
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\cli-64.exe to D:\repo\test-project\.venv\Lib\site-packages\setuptools\cli-64.exe
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\cli-arm64.exe to D:\repo\test-project\.venv\Lib\site-packages\setuptools\cli-arm64.exe
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\cli.exe to D:\repo\test-project\.venv\Lib\site-packages\setuptools\cli.exe
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\alias.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\alias.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\bdist_egg.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\bdist_egg.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\bdist_rpm.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\bdist_rpm.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\build.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\build.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\build_clib.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\build_clib.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\build_ext.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\build_ext.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\build_py.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\build_py.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\develop.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\develop.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\dist_info.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\dist_info.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\easy_install.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\easy_install.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\editable_wheel.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\editable_wheel.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\egg_info.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\egg_info.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\install.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\install.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\install_egg_info.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\install_egg_info.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\install_lib.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\install_lib.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\install_scripts.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\install_scripts.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\launcher manifest.xml to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\launcher manifest.xml
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\register.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\register.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\rotate.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\rotate.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\saveopts.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\saveopts.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\sdist.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\sdist.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\setopt.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\setopt.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\test.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\test.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\upload.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\upload.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\upload_docs.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\upload_docs.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\_requirestxt.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\_requirestxt.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\command\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\command\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\compat to D:\repo\test-project\.venv\Lib\site-packages\setuptools\compat
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\compat\py310.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\compat\py310.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\compat\py311.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\compat\py311.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\compat\py39.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\compat\py39.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\compat\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\compat\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\config to D:\repo\test-project\.venv\Lib\site-packages\setuptools\config
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\config\expand.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\config\expand.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\config\pyprojecttoml.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\config\pyprojecttoml.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\config\setupcfg.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\config\setupcfg.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\config\_apply_pyprojecttoml.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\config\_apply_pyprojecttoml.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\config\_validate_pyproject to D:\repo\test-project\.venv\Lib\site-packages\setuptools\config\_validate_pyproject
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\config\_validate_pyproject\error_reporting.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\config\_validate_pyproject\error_reporting.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\config\_validate_pyproject\extra_validations.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\config\_validate_pyproject\extra_validations.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\config\_validate_pyproject\fastjsonschema_exceptions.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\config\_validate_pyproject\fastjsonschema_exceptions.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\config\_validate_pyproject\fastjsonschema_validations.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\config\_validate_pyproject\fastjsonschema_validations.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\config\_validate_pyproject\formats.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\config\_validate_pyproject\formats.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\config\_validate_pyproject\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\config\_validate_pyproject\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\config\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\config\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\depends.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\depends.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\dep_util.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\dep_util.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\discovery.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\discovery.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\dist.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\dist.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\errors.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\errors.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\extension.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\extension.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\extern to D:\repo\test-project\.venv\Lib\site-packages\setuptools\extern
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\extern\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\extern\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\glob.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\glob.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\gui-32.exe to D:\repo\test-project\.venv\Lib\site-packages\setuptools\gui-32.exe
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\gui-64.exe to D:\repo\test-project\.venv\Lib\site-packages\setuptools\gui-64.exe
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\gui-arm64.exe to D:\repo\test-project\.venv\Lib\site-packages\setuptools\gui-arm64.exe
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\gui.exe to D:\repo\test-project\.venv\Lib\site-packages\setuptools\gui.exe
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\installer.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\installer.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\launch.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\launch.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\logging.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\logging.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\modified.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\modified.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\monkey.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\monkey.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\msvc.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\msvc.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\namespaces.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\namespaces.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\package_index.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\package_index.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\sandbox.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\sandbox.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\script (dev).tmpl to D:\repo\test-project\.venv\Lib\site-packages\setuptools\script (dev).tmpl
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\script.tmpl to D:\repo\test-project\.venv\Lib\site-packages\setuptools\script.tmpl
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\unicode_utils.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\unicode_utils.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\version.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\version.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\warnings.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\warnings.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\wheel.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\wheel.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\windows_support.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\windows_support.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_core_metadata.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_core_metadata.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\archive_util.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\archive_util.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\bcppcompiler.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\bcppcompiler.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\ccompiler.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\ccompiler.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\cmd.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\cmd.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\bdist.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\bdist.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\bdist_dumb.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\bdist_dumb.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\bdist_rpm.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\bdist_rpm.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\build.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\build.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\build_clib.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\build_clib.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\build_ext.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\build_ext.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\build_py.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\build_py.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\build_scripts.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\build_scripts.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\check.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\check.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\clean.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\clean.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\config.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\config.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\install.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\install.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\install_data.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\install_data.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\install_egg_info.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\install_egg_info.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\install_headers.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\install_headers.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\install_lib.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\install_lib.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\install_scripts.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\install_scripts.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\register.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\register.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\sdist.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\sdist.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\upload.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\upload.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\_framework_compat.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\_framework_compat.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\command\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\command\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\compat to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\compat
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\compat\py38.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\compat\py38.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\compat\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\compat\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\config.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\config.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\core.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\core.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\cygwinccompiler.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\cygwinccompiler.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\debug.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\debug.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\dep_util.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\dep_util.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\dir_util.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\dir_util.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\dist.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\dist.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\errors.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\errors.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\extension.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\extension.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\fancy_getopt.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\fancy_getopt.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\filelist.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\filelist.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\file_util.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\file_util.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\log.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\log.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\msvc9compiler.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\msvc9compiler.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\msvccompiler.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\msvccompiler.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\py38compat.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\py38compat.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\py39compat.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\py39compat.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\spawn.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\spawn.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\sysconfig.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\sysconfig.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\text_file.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\text_file.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\unixccompiler.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\unixccompiler.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\util.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\util.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\version.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\version.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\versionpredicate.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\versionpredicate.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\zosccompiler.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\zosccompiler.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\_collections.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\_collections.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\_functools.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\_functools.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\_itertools.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\_itertools.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\_log.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\_log.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\_macos_compat.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\_macos_compat.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\_modified.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\_modified.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\_msvccompiler.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\_msvccompiler.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_distutils\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_distutils\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_entry_points.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_entry_points.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_imp.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_imp.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_importlib.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_importlib.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_itertools.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_itertools.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_normalization.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_normalization.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_path.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_path.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_reqs.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_reqs.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\backports to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\backports
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\backports\tarfile.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\backports\tarfile.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\backports\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\backports\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_metadata to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_metadata
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_metadata\py.typed to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_metadata\py.typed
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_metadata\_adapters.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_metadata\_adapters.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_metadata\_collections.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_metadata\_collections.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_metadata\_compat.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_metadata\_compat.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_metadata\_functools.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_metadata\_functools.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_metadata\_itertools.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_metadata\_itertools.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_metadata\_meta.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_metadata\_meta.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_metadata\_py39compat.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_metadata\_py39compat.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_metadata\_text.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_metadata\_text.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_metadata\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_metadata\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_resources to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_resources
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_resources\abc.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_resources\abc.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_resources\py.typed to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_resources\py.typed
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_resources\readers.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_resources\readers.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_resources\simple.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_resources\simple.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_resources\_adapters.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_resources\_adapters.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_resources\_common.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_resources\_common.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_resources\_compat.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_resources\_compat.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_resources\_itertools.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_resources\_itertools.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_resources\_legacy.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_resources\_legacy.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\importlib_resources\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\importlib_resources\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\jaraco to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\jaraco
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\jaraco\context.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\jaraco\context.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\jaraco\functools to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\jaraco\functools
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\jaraco\functools\py.typed to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\jaraco\functools\py.typed
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\jaraco\functools\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\jaraco\functools\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\jaraco\functools\__init__.pyi to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\jaraco\functools\__init__.pyi
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\jaraco\text to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\jaraco\text
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\jaraco\text\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\jaraco\text\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\jaraco\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\jaraco\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\more_itertools to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\more_itertools
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\more_itertools\more.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\more_itertools\more.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\more_itertools\more.pyi to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\more_itertools\more.pyi
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\more_itertools\py.typed to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\more_itertools\py.typed
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\more_itertools\recipes.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\more_itertools\recipes.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\more_itertools\recipes.pyi to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\more_itertools\recipes.pyi
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\more_itertools\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\more_itertools\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\more_itertools\__init__.pyi to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\more_itertools\__init__.pyi
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\ordered_set.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\ordered_set.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\packaging to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\packaging
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\packaging\markers.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\packaging\markers.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\packaging\metadata.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\packaging\metadata.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\packaging\py.typed to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\packaging\py.typed
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\packaging\requirements.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\packaging\requirements.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\packaging\specifiers.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\packaging\specifiers.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\packaging\tags.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\packaging\tags.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\packaging\utils.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\packaging\utils.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\packaging\version.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\packaging\version.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\packaging\_elffile.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\packaging\_elffile.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\packaging\_manylinux.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\packaging\_manylinux.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\packaging\_musllinux.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\packaging\_musllinux.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\packaging\_parser.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\packaging\_parser.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\packaging\_structures.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\packaging\_structures.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\packaging\_tokenizer.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\packaging\_tokenizer.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\packaging\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\packaging\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\tomli to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\tomli
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\tomli\py.typed to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\tomli\py.typed
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\tomli\_parser.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\tomli\_parser.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\tomli\_re.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\tomli\_re.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\tomli\_types.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\tomli\_types.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\tomli\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\tomli\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\typing_extensions.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\typing_extensions.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\zipp.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\zipp.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\_vendor\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\_vendor\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\setuptools\__init__.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools-69.5.1.dist-info to D:\repo\test-project\.venv\Lib\site-packages\setuptools-69.5.1.dist-info
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools-69.5.1.dist-info\entry_points.txt to D:\repo\test-project\.venv\Lib\site-packages\setuptools-69.5.1.dist-info\entry_points.txt
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools-69.5.1.dist-info\LICENSE to D:\repo\test-project\.venv\Lib\site-packages\setuptools-69.5.1.dist-info\LICENSE
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools-69.5.1.dist-info\METADATA to D:\repo\test-project\.venv\Lib\site-packages\setuptools-69.5.1.dist-info\METADATA
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools-69.5.1.dist-info\RECORD to D:\repo\test-project\.venv\Lib\site-packages\setuptools-69.5.1.dist-info\RECORD
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools-69.5.1.dist-info\top_level.txt to D:\repo\test-project\.venv\Lib\site-packages\setuptools-69.5.1.dist-info\top_level.txt
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\setuptools-69.5.1.dist-info\WHEEL to D:\repo\test-project\.venv\Lib\site-packages\setuptools-69.5.1.dist-info\WHEEL
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\_distutils_hack to D:\repo\test-project\.venv\Lib\site-packages\_distutils_hack
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\_distutils_hack\override.py to D:\repo\test-project\.venv\Lib\site-packages\_distutils_hack\override.py
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\_distutils_hack\__init__.py to D:\repo\test-project\.venv\Lib\site-packages\_distutils_hack\__init__.py
DEBUG Failed to open D:\repo\test-project\.venv\Lib\site-packages to update mtime: Access is denied. (os error 5)
DEBUG Extracted 5 files name="setuptools"
DEBUG No entrypoints name="setuptools"
DEBUG No data name="setuptools"
DEBUG Writing extra metadata name="setuptools"
DEBUG Writing record name="setuptools"
Installed 1 package in 2.42s
 + setuptools==69.5.1
DEBUG Sending warning alert CloseNotify
```
  
</details>

---

_Merged by @charliermarsh on 2024-05-13 16:03_

---

_Closed by @charliermarsh on 2024-05-13 16:03_

---

_Comment by @charliermarsh on 2024-05-13 16:03_

Thank you.

---
