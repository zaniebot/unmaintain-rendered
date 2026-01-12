```yaml
number: 11100
title: "\"uv add\" fails on a private Artifactory index when a package is not available for another platform"
type: issue
state: open
author: vgalin
labels:
  - bug
assignees: []
created_at: 2025-01-30T16:14:01Z
updated_at: 2025-02-03T10:45:25Z
url: https://github.com/astral-sh/uv/issues/11100
synced_at: 2026-01-12T16:00:28Z
```

# "uv add" fails on a private Artifactory index when a package is not available for another platform

---

_@vgalin_

### Summary

I'm trying to use `uv` with a private Artifactory index. To do so, I'm using the following environment variable: 
```text
UV_INDEX : http://USER:TOKEN@REDACTED_ARTIFACTORY_URL/artifactory/api/pypi/python/simple/
```
This url is the same one I'm using within `pip.conf`.

Then when I'm trying to, for example, `uv add numpy`, I get a 404 on a macosx package. The thing is that I'm on a Windows system.

``` shell
> uv add numpy
⠙ numpy==2.2.2
error: Failed to fetch: `http://REDACTED_ARTIFACTORY_URL/artifactory/api/pypi/python/packages/packages/70/2a/69033dc22d981ad21325314f8357438078f5c28310a6d89fb3833030ec8a/numpy-2.2.2-cp310-cp310-macosx_10_9_x86_64.whl#sha256=7079129b64cb78bdc8d611d1fd7e8002c0a2565da6a47c4df8062349fee90e3e`
  Caused by: HTTP status client error (404 Not Found) for url (http://REDACTED_ARTIFACTORY_URL/artifactory/api/pypi/python/packages/packages/70/2a/69033dc22d981ad21325314f8357438078f5c28310a6d89fb3833030ec8a/numpy-2.2.2-cp310-cp310-macosx_10_9_x86_64.whl#sha256=7079129b64cb78bdc8d611d1fd7e8002c0a2565da6a47c4df8062349fee90e3e)
```

If I use a web browser to go to the following address:
http://REDACTED_ARTIFACTORY_URL/artifactory/api/pypi/python/simple/numpy/

I get a list of available Wheels for numpy. Every line is supposed to be a download link to a given version. Here is an excerpt of this page: 

```
[...]
numpy-2.2.2-cp310-cp310-macosx_10_9_x86_64.whl
numpy-2.2.2-cp310-cp310-macosx_11_0_arm64.whl
numpy-2.2.2-cp310-cp310-macosx_14_0_arm64.whl
numpy-2.2.2-cp310-cp310-macosx_14_0_x86_64.whl
numpy-2.2.2-cp310-cp310-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
numpy-2.2.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
numpy-2.2.2-cp310-cp310-musllinux_1_2_aarch64.whl
numpy-2.2.2-cp310-cp310-musllinux_1_2_x86_64.whl
numpy-2.2.2-cp310-cp310-win32.whl
numpy-2.2.2-cp310-cp310-win_amd64.whl
[...]
```

When clicking on `numpy-2.2.2-cp310-cp310-win_amd64.whl`, it starts a download on my browser.
However, on maxosx and *linux links, it just returns the following raw data :

``` json
{
  "errors" : [ {
    "status" : 404,
    "message" : "Could not find resource"
  } ]
}
```
If I had to guess, we don't use these platforms, there is no need to serve macosx/*linux packages throught Artifactory, so these files just doesn't exist on the index.

To install this package, I can proceed manually like so :

Copy the download link to numpy-2.2.2-cp310-cp310-win_amd64.whl (that can be found on the page mentionned before) and use `uv add` on it :

``` shell
>uv add http://REDACTED_ARTIFACTORY_URL/artifactory/api/pypi/python/packages/packages/92/9b/95678092febd14070cfb7906ea7932e71e9dd5a6ab3ee948f9ed975e905d/numpy-2.2.2-cp310-cp310-win_amd64.whl#sha256=64bd6e1762cd7f0986a740fee4dff927b9ec2c5e4d9a28d056eb17d332158014
Using CPython 3.10.2 interpreter at: D:\...\Python310\python.exe
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Resolved 2 packages in 46ms
░░░░░░░░░░░░░░░░░░░░ [0/1] Installing wheels...
warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 1 package in 4.07s
 + numpy==2.2.2 (from http://REDACTED_ARTIFACTORY_URL/artifactory/api/pypi/python/packages/packages/92/9b/95678092febd14070cfb7906ea7932e71e9dd5a6ab3ee948f9ed975e905d/numpy-2.2.2-cp310-cp310-win_amd64.whl)
```

Then the package can be used normally. However, as you can guess, this workaround is not very practical when this happens on multiple packages when uv is trying to install a dependency tree.

### Platform

Win Server 2016 Standard

### Version

uv 0.5.25 (9c07c3fc5 2025-01-28)

### Python version

Python 3.10.2

---

_Label `bug` added by @vgalin on 2025-01-30 16:14_

---

_Comment by @zanieb on 2025-01-30 16:16_

Why are they being listed on the index if they're not available? That seems wrong on their end.

Anyway, you can limit to the platforms you want to support: https://docs.astral.sh/uv/concepts/projects/config/#limited-resolution-environments

---

_Comment by @vgalin on 2025-02-03 10:45_

Thank you for your answer. Listing unavailable packages doesn't seem right, indeed. I don't know if it is usual for Artifactory indexes to do so.

I tried limiting platforms using `sys_platform` but it doesn't seem to change anything : the error on the maxosx macosx package 404'ing still appears.

---
