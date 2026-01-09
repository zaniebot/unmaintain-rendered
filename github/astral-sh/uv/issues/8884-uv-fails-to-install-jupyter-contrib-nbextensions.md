---
number: 8884
title: "uv fails to install `jupyter_contrib_nbextensions==0.7.0` while pip succeeds"
type: issue
state: closed
author: leodevian
labels:
  - windows
assignees: []
created_at: 2024-11-07T09:48:01Z
updated_at: 2025-07-24T10:07:51Z
url: https://github.com/astral-sh/uv/issues/8884
synced_at: 2026-01-07T13:12:18-06:00
---

# uv fails to install `jupyter_contrib_nbextensions==0.7.0` while pip succeeds

---

_Issue opened by @leodevian on 2024-11-07 09:48_

Hello,

I'm using uv 0.4.30 (61ed2a236 2024-11-04) on Windows and I'm trying to install `jupyter_contrib_nbextensions==0.7.0`. It seems that uv fails to install the package as it cannot be built with the latest versions of `setuptools`, due to some deprecations.

```shell
uv pip install jupyter_contrib_nbextensions==0.7.0
```

However, the latest version of pip (24.3.1) succeeds to build and install the package and I would like to understand this difference in behavior.

```shell
pip install jupyter_contrib_nbextensions==0.7.0
```

I read the documentation on build failures and I don't understand if I can force a version of `setuptools` and `wheel` for `jupyter_contrib_nbextensions`.

Also, can I manage this package with uv if it has been installed using pip?

Thank you for your awesome work!

---

_Comment by @FishAlchemist on 2024-11-07 10:05_

I'm a Windows user and I couldn't reproduce the problem you mentioned. 
~~Furthermore, according to your description, the package installed with uv is not the same as the one installed with pip.~~ (Corrected)

---

_Comment by @FishAlchemist on 2024-11-07 12:48_

@leodevian 
I suggest you provide more complete information. After all, if we can't reproduce the issue, we won't know what the error message is. As for the method, at least add ``--verbose`` to get complete information.

---

_Comment by @leodevian on 2024-11-07 13:07_

Thanks for your quick feedback! Indeed, I did mispell the package name in the description (now edited).

The following command should reproduce the issue:

```shell
mkdir issue
cd issue
uv venv -p 3.11 *> log.txt
uv pip install jupyter_contrib_nbextensions==0.7.0 -vvv *>> log.txt
```

I'm using cpython-3.11.10-windows-x86_64-none, which has been installed using uv.

Here are the logs with `--verbose` (I'm not used to redirection and struggled to get them).

[log.txt](https://github.com/user-attachments/files/17661915/log.txt)

---

_Comment by @FishAlchemist on 2024-11-07 13:13_

@leodevian Could you provide the ``pyproject.toml``  that can reproduce this issue? Because when I tried your problem, I used a default pyproject.toml.

Edit:
But it has a lot of warnings.

---

_Comment by @leodevian on 2024-11-07 13:19_

@FishAlchemist I don't have `pyproject.toml` in my directory.

I tried the following commands and it still failed with the same error:

```powershell
mkdir issue
cd issue
uv init -p 3.11
uv add jupyter_contrib_nbextensions==0.7.0
```

It seems that pip succeeds to build a wheel while uv fails?

EDIT: I don't understand the difference in behavior. Does pip support deprecated methods that have not been implemented in uv?

---

_Label `needs-mre` added by @charliermarsh on 2024-11-07 13:21_

---

_Comment by @charliermarsh on 2024-11-07 13:25_

Are you perhaps hitting the filename length limit on Windows?

---

_Comment by @leodevian on 2024-11-07 13:30_

The directory is located in `%USERPROFILE%`, which is 18 characters long, so I doubt it.

I will try to reproduce the issue on my personal computer later, to investigate further.

---

_Comment by @FishAlchemist on 2024-11-07 13:31_

> Are you perhaps hitting the filename length limit on Windows?

It seems possible, as my computer enable Long Paths.

To check if it's enabled, you can simply open PowerShell to query. Reading the registry doesn't require administrative privileges.
```
Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" `
∙ -Name "LongPathsEnabled"
```

---

_Comment by @charliermarsh on 2024-11-07 13:33_

Yes but the error you shared is for `build\lib\jupyter_contrib_nbextensions\nbextensions\code_prettify\README_code_prettify.md` which will already be nested inside the build cache directory.

---

_Comment by @leodevian on 2024-11-07 13:37_

I don't have it and I have no admin rights on this computer. I will give your more information once I tried to reproduce the issue on my personal computer (with admin rights).

Could it be that building universal wheels is not supported by uv? To me, it seems that this feature has been deprecated for quite some time...

From the logs:

```text
With Python 2.7 end-of-life, support for building universal wheels
        (i.e., wheels that support both Python 2 and Python 3)
        is being obviated.
```

---

_Comment by @FishAlchemist on 2024-11-07 13:39_

> Yes but the error you shared is for `build\lib\jupyter_contrib_nbextensions\nbextensions\code_prettify\README_code_prettify.md` which will already be nested inside the build cache directory.

@charliermarsh Yes, I just closed the long path, and then the problem occurred

Edit:
Can UV  detect long paths that are not enabled during the build phase?

----------

@leodevian This is the configuration method provided by Microsoft.
https://learn.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?tabs=powershell#registry-setting-to-enable-long-paths

---

_Label `needs-mre` removed by @charliermarsh on 2024-11-07 13:46_

---

_Label `windows` added by @charliermarsh on 2024-11-07 13:46_

---

_Comment by @leodevian on 2024-11-07 14:03_

@FishAlchemist Thanks a lot! I guess that's good news.

It pains me because I'm promoting uv within my organization and no one will be able to disable the maximum file path limitation because of internal policies...

---

_Comment by @leodevian on 2024-11-07 16:27_

Is it possible to bypass the limitation using `\\?\`?

Here is how I would do it using Python.

```python
import os

longpath = "..."

longpath = os.path.realpath(longpath)

unc = "\\\\?\\" if len(longpath) >= 260 else ""

os.mkdir(unc + longpath)

with open(unc + os.path.join(longpath, "hello.txt"), "w") as f:
    f.write("Hello world!")
```


---

_Comment by @charliermarsh on 2024-11-07 16:40_

I don't believe so. Using those UNC paths breaks a lot of things -- for example, any relative paths in the build will be interpreted relative to the drive: https://github.com/astral-sh/uv/pull/1277

---

_Comment by @leodevian on 2024-11-07 17:01_

Well, I had mitigated hopes on this... I hope that I won't encounter this issue too many times. Meanwhile, I'll look for a workaround.

Is it possible to install a package using pip and still manage it using uv (so it won't be uninstalled with `uv sync` or `uv add`)?

---

_Comment by @charliermarsh on 2024-11-07 17:09_

I'll look into reducing the length of the cache path.

---

_Comment by @charliermarsh on 2024-11-07 18:10_

I actually don't really understand why this succeeds with pip but not uv. Their build cache prefix isn't any shorter, and even when I set `--cache-dir` to something short, uv still fails.

---

_Comment by @FishAlchemist on 2024-11-07 19:14_

@charliermarsh  I've tracked program's behavior, and it should serve as a reference.
The nesting of the UV cache seems quite deep, but pip seems to have only the nesting of the source code.
# UV (uv 0.4.30 (61ed2a236 2024-11-04))
( maximum 233 without user home path)
```
~\AppData\Local\Temp\.tmpyBcss8\sdists-v5\pypi\jupyter-contrib-nbextensions\0.7.0\Jk8_nRpxZF8Zdq-pjfEJr\jupyter_contrib_nbextensions-0.7.0.tar.gz\build\lib\jupyter_contrib_nbextensions\nbextensions\code_prettify\README_code_prettify.md
```
# PIP (pip 24.3.1)
(maximum 209 without user home path)
```
~\AppData\Local\Temp\pip-install-7pgurthd\jupyter-contrib-nbextensions_0d7b5c639ab74a4fb664a5235eb7c74d\src\jupyter_contrib_nbextensions\nbextensions\code_prettify\README_code_prettify.md
~\AppData\Local\Temp\pip-install-7pgurthd\jupyter-contrib-nbextensions_0d7b5c639ab74a4fb664a5235eb7c74d\build\lib\jupyter_contrib_nbextensions\nbextensions\code_prettify\README_code_prettify.md
~\AppData\Local\Temp\pip-install-7pgurthd\jupyter-contrib-nbextensions_0d7b5c639ab74a4fb664a5235eb7c74d\build\bdist.win-amd64\wheel\jupyter_contrib_nbextensions\nbextensions\code_prettify\README_code_prettify.md
```

---

_Comment by @konstin on 2024-11-07 19:31_

Collecting clues:

With `pip install jupyter_contrib_nbextensions==0.7.0`:

```
  copying build\lib\jupyter_contrib_nbextensions\nbextensions\codefolding\main.js -> build\bdist.win-amd64\wheel\.\jupyter_contrib_nbextensions\nbextensions\codefolding
  copying build\lib\jupyter_contrib_nbextensions\nbextensions\codefolding\readme.md -> build\bdist.win-amd64\wheel\.\jupyter_contrib_nbextensions\nbextensions\codefolding
  creating build\bdist.win-amd64\wheel\jupyter_contrib_nbextensions\nbextensions\codemirror_mode_extensions
  copying build\lib\jupyter_contrib_nbextensions\nbextensions\codemirror_mode_extensions\codemirror_mode_extensions.yaml -> build\bdist.win-amd64\wheel\.\jupyter_contrib_nbextensions\nbextensions\codemirror_mode_extensions
```

With `uv pip install jupyter_contrib_nbextensions==0.7.0`, but also with `uv pip install jupyter_contrib_nbextensions==0.7.0 --cache-dir C:\Users\Konsti\a`:

```
copying src\jupyter_contrib_nbextensions\nbextensions\codefolding\main.js -> build\lib\jupyter_contrib_nbextensions\nbextensions\codefolding
copying src\jupyter_contrib_nbextensions\nbextensions\codefolding\readme.md -> build\lib\jupyter_contrib_nbextensions\nbextensions\codefolding
creating build\lib\jupyter_contrib_nbextensions\nbextensions\codemirror_mode_extensions
copying src\jupyter_contrib_nbextensions\nbextensions\codemirror_mode_extensions\codemirror_mode_extensions.yaml -> build\lib\jupyter_contrib_nbextensions\nbextensions\codemirror_mode_extensions
```

With `uv pip install jupyter_contrib_nbextensions==0.7.0 --cache-dir asdf` in `C:\Users\Konsti\projects\uv\`:

```
copying src\jupyter_contrib_nbextensions\nbextensions\codefolding\main.js -> build\lib\jupyter_contrib_nbextensions\nbextensions\codefolding
copying src\jupyter_contrib_nbextensions\nbextensions\codefolding\readme.md -> build\lib\jupyter_contrib_nbextensions\nbextensions\codefolding
creating build\lib\jupyter_contrib_nbextensions\nbextensions\codemirror_mode_extensions
copying src\jupyter_contrib_nbextensions\nbextensions\codemirror_mode_extensions\codemirror_mode_extensions.yaml -> build\lib\jupyter_contrib_nbextensions\nbextensions\codemirror_mode_extensions
copying src\jupyter_contrib_nbextensions\nbextensions\codemirror_mode_extensions\main.js -> build\lib\jupyter_contrib_nbextensions\nbextensions\codemirror_mode_extensions
creating build\lib\jupyter_contrib_nbextensions\nbextensions\collapsible_headings
copying src\jupyter_contrib_nbextensions\nbextensions\collapsible_headings\collapsible_headings.yaml -> build\lib\jupyter_contrib_nbextensions\nbextensions\collapsible_headings
copying src\jupyter_contrib_nbextensions\nbextensions\collapsible_headings\icon.png -> build\lib\jupyter_contrib_nbextensions\nbextensions\collapsible_headings
copying src\jupyter_contrib_nbextensions\nbextensions\collapsible_headings\main.css -> build\lib\jupyter_contrib_nbextensions\nbextensions\collapsible_headings
copying src\jupyter_contrib_nbextensions\nbextensions\collapsible_headings\main.js -> build\lib\jupyter_contrib_nbextensions\nbextensions\collapsible_headings
copying src\jupyter_contrib_nbextensions\nbextensions\collapsible_headings\readme.md -> build\lib\jupyter_contrib_nbextensions\nbextensions\collapsible_headings
copying src\jupyter_contrib_nbextensions\nbextensions\collapsible_headings\screenshot.png -> build\lib\jupyter_contrib_nbextensions\nbextensions\collapsible_headings
creating build\lib\jupyter_contrib_nbextensions\nbextensions\comment-uncomment
copying src\jupyter_contrib_nbextensions\nbextensions\comment-uncomment\comment-uncomment.yaml -> build\lib\jupyter_contrib_nbextensions\nbextensions\comment-uncomment
copying src\jupyter_contrib_nbextensions\nbextensions\comment-uncomment\icon.png -> build\lib\jupyter_contrib_nbextensions\nbextensions\comment-uncomment
copying src\jupyter_contrib_nbextensions\nbextensions\comment-uncomment\main.js -> build\lib\jupyter_contrib_nbextensions\nbextensions\comment-uncomment
copying src\jupyter_contrib_nbextensions\nbextensions\comment-uncomment\readme.md -> build\lib\jupyter_contrib_nbextensions\nbextensions\comment-uncomment
creating build\lib\jupyter_contrib_nbextensions\nbextensions\contrib_nbextensions_help_item
copying src\jupyter_contrib_nbextensions\nbextensions\contrib_nbextensions_help_item\README.md -> build\lib\jupyter_contrib_nbextensions\nbextensions\contrib_nbextensions_help_item
copying src\jupyter_contrib_nbextensions\nbextensions\contrib_nbextensions_help_item\contrib_nbextensions_help_item.yaml -> build\lib\jupyter_contrib_nbextensions\nbextensions\contrib_nbextensions_help_item
```

I can also `uv build` in the repo

---

_Comment by @charliermarsh on 2024-11-07 21:19_

@konstin -- You may need to use `--use-pep517` in `pip install` (it still failed for me, but I think the behavior may be different).

---

_Comment by @charliermarsh on 2024-11-07 21:33_

Oh my guess is the problem is this huge prefix? `sdists-v5\pypi\jupyter-contrib-nbextensions\0.7.0\Jk8_nRpxZF8Zdq-pjfEJr\jupyter_contrib_nbextensions-0.7.0.tar.` I need to trace this.

---

_Comment by @charliermarsh on 2024-11-07 21:39_

So sketching this out... When we go to install that wheel:

1. We create a new revision in the cache, like `${UV_CACHE_DIR}/sdists-v5/pypi/jupyter-contrib-nbextensions/0.7.0/Jk8_nRpxZF8Zdq-pjfEJr`.
2. We stream-unzip the contents into a temporary directory within `${UV_CACHE_DIR}/sdists-v5`, like `${UV_CACHE_DIR}/sdists-v5/.tmpyBcss8`.
3. We move the temporary directory to into the `sdists-v5` bucket, e.g., we move it to `${UV_CACHE_DIR}/sdists-v5/pypi/jupyter-contrib-nbextensions/0.7.0/Jk8_nRpxZF8Zdq-pjfEJr/jupyter_contrib_nbextensions-0.7.0.tar.gz` (this is a _directory_, despite the filename).
4. We then build the source that has been extracted to `${UV_CACHE_DIR}/sdists-v5/pypi/jupyter-contrib-nbextensions/0.7.0/Jk8_nRpxZF8Zdq-pjfEJr/jupyter_contrib_nbextensions-0.7.0.tar.gz`.
5. To build, we create a virtual environment in a temporary directory in `${UV_CACHE_DIR}/builds-v0`, like `${UV_CACHE_DIR}/builds-v0/.tmay5cs91`.
6. To build, we create a temporary directory within the target directory... So, like: `${UV_CACHE_DIR}/sdists-v5/pypi/jupyter-contrib-nbextensions/0.7.0/Jk8_nRpxZF8Zdq-pjfEJr/.tmpQWxTfb`. Then we build the wheel using and into that directory (i.e., it's the output directory passed to the PEP 517 hooks).
7. We then rename the built wheel them to, e.g., `${UV_CACHE_DIR}/sdists-v5/pypi/jupyter-contrib-nbextensions/0.7.0/Jk8_nRpxZF8Zdq-pjfEJr/interpreters_pep_734-0.4.0-py3-none-any.whl` (this is a file).
9. We then unzip the built distribution:
    - First, we create a temporary direcotry in the cache root (e.g., `${UV_CACHE_DIR}/.tmay5cs91`).
    - Then we unzip into that directory.
    - Then we persist it in the archive bucket (e.g., we rename it to `${UV_CACHE_DIR}/archive-v0/5ZQyJUPRJyOe8jCGM3gql`).
    - Then we create a symlink from the archive bucket to the `sdists-v5` bucket (e.g., `${UV_CACHE_DIR}/sdists-v5/pypi/jupyter-contrib-nbextensions/0.7.0/Jk8_nRpxZF8Zdq-pjfEJr/interpreters_pep_734-0.4.0-py3-none-any`).


---

_Comment by @charliermarsh on 2024-11-07 21:52_

I think one thing we should _definitely_ change is that we shouldn't build the wheel into `${UV_CACHE_DIR}/sdists-v5/pypi/jupyter-contrib-nbextensions/0.7.0/Jk8_nRpxZF8Zdq-pjfEJr/.tmpQWxTfb`. That's just a huge path and there's no reason for it? I'll do that first.

---

_Comment by @charliermarsh on 2024-11-07 21:53_

We may also want to consider shortening `${UV_CACHE_DIR}/sdists-v5/pypi/jupyter-contrib-nbextensions/0.7.0/Jk8_nRpxZF8Zdq-pjfEJr`. It's just a huge path. And if you use a non-PyPI index, it's even longer because the `pypi` segment gets expanded to, e.g., `index/6934cf8789bf06c0`.

---

_Comment by @charliermarsh on 2024-11-07 23:08_

Okay cool, I just got this package to build on my machine. It'll be a series of small PRs.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-07 23:12_

---

_Referenced in [astral-sh/uv#8905](../../astral-sh/uv/pulls/8905.md) on 2024-11-08 00:09_

---

_Referenced in [astral-sh/uv#8907](../../astral-sh/uv/pulls/8907.md) on 2024-11-08 00:25_

---

_Closed by @charliermarsh on 2024-11-08 00:50_

---

_Comment by @MitchellAcoustics on 2025-07-24 10:07_

It looks like I am hitting this same issue:

```
uv add --group docs "jupyter_contrib_nbextensions >= 0.5.1, <1" -vvv *>> log.txt
```
produces a very similar error as @leodevian . I also don't have admin rights and can't enable long paths.

uv version is `uv 0.7.21 (77c771c7f 2025-07-14)`

It looks like the bug fix isn't working for me - any suggestions for another way around this?

[log.txt](https://github.com/user-attachments/files/21405869/log.txt)

---
