---
number: 6072
title: "error: Failed to install cpython-3.12.4-macos-aarch64-none: Request failed after 3 retries"
type: issue
state: closed
author: pdpark
labels:
  - question
assignees: []
created_at: 2024-08-13T23:18:17Z
updated_at: 2025-08-28T10:23:33Z
url: https://github.com/astral-sh/uv/issues/6072
synced_at: 2026-01-07T13:12:17-06:00
---

# error: Failed to install cpython-3.12.4-macos-aarch64-none: Request failed after 3 retries

---

_Issue opened by @pdpark on 2024-08-13 23:18_

Comand: `uv python install` 
Output: `error: Failed to install cpython-3.12.4-macos-aarch64-none: Request failed after 3 retries`

Re-ran command with verbose flag and got this output (three times):
```sh
DEBUG Transient request failure for https://github.com/indygreg/python-build-standalone/releases/download/20240726/cpython-3.12.4%2B20240726-aarch64-apple-darwin-install_only_stripped.tar.gz, retrying: error sending request for url (https://github.com/indygreg/python-build-standalone/releases/download/20240726/cpython-3.12.4%2B20240726-aarch64-apple-darwin-install_only_stripped.tar.gz)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
```

UV version: `uv 0.2.36 (Homebrew 2024-08-13)`
Platform: MacOS Sonoma 14.5 on M1 Pro Apple Silicon

Notes: Python version specified in `pyproject.toml`:
```toml
[project]
requires-python = ">=3.12"
```

---

_Comment by @zanieb on 2024-08-13 23:30_

Hi! Do you have a proxy running? Are you using `UV_NATIVE_TLS`?

---

_Label `question` added by @zanieb on 2024-08-13 23:30_

---

_Comment by @pdpark on 2024-08-13 23:54_

Not using UV_NATIVE_TLS.

Looks like it may be a proxy issue. Will have to fall back to pyenv for now. 

Thanks.

---

_Comment by @zanieb on 2024-08-14 00:09_

I can only presume you are able to access GitHub normally? You can set `UV_NATIVE_TLS=true` or use the `--native-tls` flag if you need to use your system certificates with uv. We do not use the system certificate store by default, but I presume you're using a proxy with a certificate that is registered with your system.

---

_Comment by @pdpark on 2024-08-14 00:09_

Just a suggestion since this may be an issue for others: instead of downloading pre-compiled versions of python you could build python locally like `pyenv` does. This zig project could make that a lot easier, I think (and, likely, faster): `https://github.com/allyourcodebase/cpython`. It would likely require significant contributions to mature that project to the point where it could act as a full replacement for `pyenv`, but it would be a great alternative.

---

_Comment by @zanieb on 2024-08-14 00:11_

It's going to be _way_ slower to locally build optimized versions of Python — thats why we offer pre-built distributions. They're hosted on GitHub and you can use `UV_PYTHON_INSTALL_MIRROR` if you need to install them from a different host .

---

_Comment by @pdpark on 2024-08-14 00:21_

Yep, of course! I meant faster than pyenv 😄.

Here's a great discussion about how pre-building Python wheels for PyPI (not exactly the same issue, but related...) is getting out of hand with the number of build targets exploding, and how the zig build system could be a better option: https://www.youtube.com/watch?v=i9nFvSpcCzo

Thanks for your help!

---

_Comment by @zanieb on 2024-08-14 00:24_

No problem. Feel free to let us know if you have any more problems.

---

_Closed by @zanieb on 2024-08-14 00:24_

---

_Comment by @pdpark on 2024-08-14 00:31_

> Hi! Do you have a proxy running? Are you using `UV_NATIVE_TLS`?

BTW I tried again this time using the `--native-tls` flag and it worked!

Interesting that it installed Python `3.12.4`, though. Python `3.12.5` is the latest version of `3.12`. It's only been out for about a week, so maybe it hasn't been compiled yet?

---

_Comment by @charliermarsh on 2024-08-14 00:44_

Oh, thank I can add it. I'm annoyed, I thought I subscribed to release notifications!

---

_Comment by @charliermarsh on 2024-08-14 01:32_

Adding here: https://github.com/indygreg/python-build-standalone/pull/299

---

_Comment by @pdpark on 2024-08-14 15:00_

FYI: this issue (`uv python` not having 12.5 available) came up on the most recent episode of Python Bytes (they're big fans of UV): https://www.youtube.com/watch?v=0Sa0z2XXwwk

---

_Comment by @Symbolk on 2025-03-24 02:40_

How did I resolve the issue by checking the uv-h: `uv venv openr1 --python 3.11 --native-tls --allow-insecure-host https://github.com`



---

_Comment by @sjwyg on 2025-04-07 14:55_

I use pip install uv, and then execute "uv add requests" to add dependency, but it didn't work. like this:

uv add requests

Using CPython 3.12.4 interpreter at: C:\Users\g00026547\.conda\envs\py312\python.exe
Creating virtual environment at: .venv
⠋ uv-rumen==0.1.0                                                                                                                                                                                   
error: Failed to fetch: `https://pypi.tuna.tsinghua.edu.cn/simple/requests/`
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://pypi.tuna.tsinghua.edu.cn/simple/requests/)
  Caused by: client error (Connect)
  Caused by: tcp connect error: 由于连接方在一段时间后没有正确答复或连接的主机没有反应，连接尝试失败。 (os error 10060)
  Caused by: 由于连接方在一段时间后没有正确答复或连接的主机没有反应，连接尝试失败。 (os error 10060)

---

_Comment by @sjwyg on 2025-04-07 14:58_

and use chrome visit "https://pypi.tuna.tsinghua.edu.cn/simple/requests/" is successful, and use pip is also successful. 
only uv can't 

i think it's a problem about proxy, but i don't know how to resolve it

---

_Comment by @Alphayellowcat on 2025-08-28 10:23_

> and use chrome visit "https://pypi.tuna.tsinghua.edu.cn/simple/requests/" is successful, and use pip is also successful. only uv can't
> 
> I think it's a problem about proxy, but I don't know how to resolve it

It should be a difference between the snapshot version and the official version, causing there to be no URL to fetch from.

---
