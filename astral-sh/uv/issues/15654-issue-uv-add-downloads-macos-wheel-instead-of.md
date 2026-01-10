---
number: 15654
title: "Issue: uv add downloads macOS wheel instead of Windows amd64 wheel on Win64 platform, while uv pip install works correctly"
type: issue
state: open
author: kermitbu
labels:
  - question
assignees: []
created_at: 2025-09-03T10:38:17Z
updated_at: 2025-09-04T23:09:00Z
url: https://github.com/astral-sh/uv/issues/15654
synced_at: 2026-01-10T01:25:58Z
---

# Issue: uv add downloads macOS wheel instead of Windows amd64 wheel on Win64 platform, while uv pip install works correctly

---

_Issue opened by @kermitbu on 2025-09-03 10:38_

### Summary

Environment
OS Platform: Windows 10 (amd64 architecture)
uv Version: [0.8.14] (Note: --platform argument is not supported in this version)
uv Configuration File: uv.toml (located at C:\Users\kermit\AppData\Roaming\uv\uv.toml) with the following content:
```toml
[[index]]
url = "https://test.pypi.org/simple"
default = true
```
Steps to Reproduce
Initialize a project using uv init mydemo (successfully creates the mydemo project with pyproject.toml).
Attempt to add polars dependency via uv add:
```bash
cd mydemo
uv add polars
```
Result: uv downloads the macOS-specific wheel (e.g., polars-1.32.0-cp39-abi3-macosx_10_12_x86_64.whl).
Install polars via uv pip install in the same project directory:
```bash
uv pip install polars
```
Result: Works correctly—downloads the Windows amd64 wheel (e.g., polars-1.32.0-cp39-abi3-win_amd64.whl) and installs successfully.
Expected Behavior
uv add should consistently detect the Windows amd64 platform (same as uv pip install) and download the corresponding Windows wheel, instead of defaulting to macOS.
Actual Behavior
uv add incorrectly resolves to macOS wheel on Windows amd64 platform, while uv pip install behaves as expected.

### Platform

windows10 amd64

### Version

uv0.8.14

### Python version

python 3.12.7

---

_Label `bug` added by @kermitbu on 2025-09-03 10:38_

---

_Comment by @konstin on 2025-09-03 11:28_

Does uv actually install the MacOS wheel, or is it only downloaded for metadata? uv in universal mode (`uv lock`, `uv sync`, `uv add`, etc.), uses a consistent wheel, usually the MacOS one to get the metadata, if the index supports neither PEP 658 nor HTTP range requests. This wheel is only used to get the dependencies of a package (from the `.dist-info/METADATA` file), but should not be installed on Windows.

---

_Label `question` added by @zanieb on 2025-09-03 11:56_

---

_Label `bug` removed by @zanieb on 2025-09-03 11:56_

---

_Comment by @kermitbu on 2025-09-04 00:46_

The following snippet is from the logs:
TRACE: Request for http://test.pypi.org/repository/pypi-public/packages/polars/1.32.0/polars-1.32.0-cp39-abi3-macosx_10_12_x86_64.whl failed with Not Found, checking for credentials
TRACE: No credentials in cache for URL http://test.pypi.org/repository/pypi-public/simple/
TRACE: No credentials in cache for realm http://test.pypi.org/
DEBUG: No netrc file found
TRACE: Considering retry of response HTTP 404 Not Found for http://test.pypi.org/repository/pypi-public/packages/polars/1.32.0/polars-1.32.0-cp39-abi3-macosx_10_12_x86_64.whl
TRACE: Cannot retry error: not an extended IO error
This issue might be caused by the absence of the macOS package in my private repository. Are there any parameters or configurations available to force the download of a package for a specific platform?

---

_Comment by @konstin on 2025-09-04 09:15_

The URL `http://test.pypi.org/repository/pypi-public/packages/polars/1.32.0/polars-1.32.0-cp39-abi3-macosx_10_12_x86_64.whl` looks off, this should not appear when setting `url = "https://test.pypi.org/simple"`, is there anything else you configure?

---

_Comment by @shauneccles on 2025-09-04 23:09_

> in my private repository

Are you redacting this private repo in this issue log by replacing it with test.pypi.org?

---
