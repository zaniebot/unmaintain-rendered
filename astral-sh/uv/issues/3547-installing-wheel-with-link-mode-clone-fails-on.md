---
number: 3547
title: Installing wheel with link-mode=clone fails on windows
type: issue
state: closed
author: bschoenmaeckers
labels:
  - bug
  - windows
assignees: []
created_at: 2024-05-13T14:43:13Z
updated_at: 2024-05-13T16:03:41Z
url: https://github.com/astral-sh/uv/issues/3547
synced_at: 2026-01-10T01:23:29Z
---

# Installing wheel with link-mode=clone fails on windows

---

_Issue opened by @bschoenmaeckers on 2024-05-13 14:43_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Installing a wheel with uv 0.1.42 fails when using `linkmode = clone` fails on a windows dev drive fails because windows does not support cloning directories as a whole. I suspect this fails on linux as well but haven't tested it myself just yet.

It currently shows the error message `the source path is not an existing regular file: Access is denied. (os error 5)
` coming from the [reflink-copy](https://github.com/cargo-bins/reflink-copy/blob/0.1.17/src/lib.rs#L76-L79) library. Uv should  fall back to cloning individual files on windows (at least) to support COW installations.

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
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
TRACE Received package metadata for: setuptools
TRACE selecting candidate for package setuptools with range Range { segments: [(Unbounded, Unbounded)] } with 557 remote versions
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "69.5.1" version
DEBUG Searching for a compatible version of setuptools (*)
TRACE selecting candidate for package setuptools with range Range { segments: [(Unbounded, Unbounded)] } with 557 remote versions
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "69.5.1" version
DEBUG Selecting: setuptools==69.5.1 (setuptools-69.5.1-py3-none-any.whl)
⠙ setuptools==69.5.1                                                                                                                                                                                                                                                                                                TRACE cached request https://files.pythonhosted.org/packages/f7/29/13965af254e3373bceae8fb9a0e6ea0d0e571171b80d6646932131d6439b/setuptools-69.5.1-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f7/29/13965af254e3373bceae8fb9a0e6ea0d0e571171b80d6646932131d6439b/setuptools-69.5.1-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: setuptools==69.5.1
INFO add_decision: setuptools @ 69.5.1
DEBUG Tried 2 versions: root 1, setuptools 1
Resolved 1 package in 5ms
DEBUG Requirement already cached: setuptools==69.5.1
DEBUG Unnecessary package: pip==24.0
░░░░░░░░░░░░░░░░░░░░ [0/1] Installing wheels...                                                                                                                                                                                                                                                                     DEBUG Extracting file name="setuptools"
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\distutils-precedence.pth to D:\repo\test-project\.venv\Lib\site-packages\distutils-precedence.pth
DEBUG Cloning \\?\D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources to D:\repo\test-project\.venv\Lib\site-packages\pkg_resources
error: Failed to install: setuptools-69.5.1-py3-none-any.http.whl (setuptools==69.5.1)
  Caused by: Failed to reflink D:\packages\uv\archive-v0\SAaq2Tm4XZ_Ep1hD_lCQB\pkg_resources to .venv\Lib\site-packages\pkg_resources
  Caused by: the source path is not an existing regular file: Access is denied. (os error 5)

```
  
</details>


---

_Comment by @charliermarsh on 2024-05-13 14:45_

Sort of mixed on this. If you request `--link-mode=clone` and your system doesn't support cloning, it seems right (IMO) to fail with an error.

---

_Comment by @bschoenmaeckers on 2024-05-13 15:44_

I kind of understand where you are coming from. It a unfortunate limitation of microsoft implementation of Copy-On-Write on ReFS.  But I still think it is a "nice to have" to support cloning on windows. 

---

_Comment by @charliermarsh on 2024-05-13 15:45_

Sorry, I misread. I do think we should support this.

---

_Comment by @charliermarsh on 2024-05-13 15:45_

I thought you were saying that cloning wasn't supported at all in Windows, and so we should fall back to copying. But you're saying cloning _directories_ isn't supported, so we should fall back to cloning the individual files. This is straightforward and we should do it.

---

_Label `bug` added by @charliermarsh on 2024-05-13 15:46_

---

_Comment by @bschoenmaeckers on 2024-05-13 15:46_

I can open a MR if I'm free to do so.

---

_Comment by @charliermarsh on 2024-05-13 15:47_

Sure. We already have fallback code in `clone_recursive` that does exactly this (I think), but it only catches directory-already-exists errors.

---

_Label `windows` added by @charliermarsh on 2024-05-13 15:47_

---

_Referenced in [astral-sh/uv#3551](../../astral-sh/uv/pulls/3551.md) on 2024-05-13 15:55_

---

_Closed by @charliermarsh on 2024-05-13 16:03_

---
