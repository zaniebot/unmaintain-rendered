---
number: 15579
title: uv python instal
type: issue
state: closed
author: classronin
labels:
  - enhancement
assignees: []
created_at: 2025-08-29T07:22:02Z
updated_at: 2025-08-30T07:36:20Z
url: https://github.com/astral-sh/uv/issues/15579
synced_at: 2026-01-10T01:25:57Z
---

# uv python instal

---

_Issue opened by @classronin on 2025-08-29 07:22_

### Summary

如何解决网络问题及无网络的备用？


```
E:\AppData\uv\cpython\20250828>uv python install 3.12.11 cpython-3.12.11+20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz
error: `cpython-3.12.11+20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz` is not a valid Python download request; see `uv help python` for supported formats and `uv python list --only-downloads` for available versions

>uv python install 3.12 -v
DEBUG uv 0.8.14 (af856fb88 2025-08-28)
DEBUG Acquired lock for `E:\AppData\uv\python`
WARN Ignoring malformed managed Python entry:
    Failed to parse Python installation key `bin`: expected exactly 5 `-`-separated values, got 1
WARN Ignoring malformed managed Python entry:
    Failed to parse Python installation key `cache`: expected exactly 5 `-`-separated values, got 1
DEBUG No installation found for request `3.12 (cpython-3.12-windows-x86_64-none)`
DEBUG Found download `cpython-3.12.11-windows-x86_64-none` for request `3.12 (cpython-3.12-windows-x86_64-none)`
DEBUG Using request timeout of 30s
DEBUG Downloading https://github.com/astral-sh/python-build-standalone/releases/download/20250828/cpython-3.12.11%2B20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz to `E:\AppData\uv\python\cache\0b8fab064-cpython-3.12.11-20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz`
DEBUG Transient request failure for https://github.com/astral-sh/python-build-standalone/releases/download/20250828/cpython-3.12.11%2B20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz, retrying: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250828/cpython-3.12.11%2B20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz)
  Caused by: client error (Connect)
  Caused by: 远程主机强迫关闭了一个现有的连接。 (os error 10054)
DEBUG Transient request failure for https://github.com/astral-sh/python-build-standalone/releases/download/20250828/cpython-3.12.11%2B20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz, retrying: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250828/cpython-3.12.11%2B20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz)
  Caused by: client error (Connect)
  Caused by: 远程主机强迫关闭了一个现有的连接。 (os error 10054)
DEBUG Transient request failure for https://github.com/astral-sh/python-build-standalone/releases/download/20250828/cpython-3.12.11%2B20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz, retrying: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250828/cpython-3.12.11%2B20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz)
  Caused by: client error (Connect)
  Caused by: 远程主机强迫关闭了一个现有的连接。 (os error 10054)
DEBUG Transient request failure for https://github.com/astral-sh/python-build-standalone/releases/download/20250828/cpython-3.12.11%2B20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz, retrying: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250828/cpython-3.12.11%2B20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz)
  Caused by: client error (Connect)
  Caused by: 远程主机强迫关闭了一个现有的连接。 (os error 10054)
error: Failed to install cpython-3.12.11-windows-x86_64-none
  Caused by: Failed to download https://github.com/astral-sh/python-build-standalone/releases/download/20250828/cpython-3.12.11%2B20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250828/cpython-3.12.11%2B20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz)
  Caused by: client error (Connect)
  Caused by: 远程主机强迫关闭了一个现有的连接。 (os error 10054)
DEBUG Released lock at `E:\AppData\uv\python\.lock`

E:\AppData\uv\cpython\20250828>
```

https://docs.astral.sh/uv/reference/environment/
uv文档下找不到本地安装已下载python的Astral.sh版本。

有什么能实现‘--only-local’？

尽管'uv venv e:\python3.12'能解决，但更喜欢uv管理安装本地python.


### Example

_No response_

---

_Label `enhancement` added by @classronin on 2025-08-29 07:22_

---

_Comment by @konstin on 2025-08-29 08:26_

Unfortunately I do not fully understand your question, are you looking for the `UV_PYTHON_CACHE_DIR` option? If you create a directory and put downloaded Pythons into it, you can use `UV_PYTHON_CACHE_DIR` to install offline.

---

_Comment by @classronin on 2025-08-29 09:19_

@konstin 

```
E:\AppData\uv\python\cache>fd
cpython-3.12.11+20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz

>echo %UV_PYTHON_CACHE_DIR%
E:\AppData\uv\python\cache

>uv python install 3.12 -v
DEBUG uv 0.8.14 (af856fb88 2025-08-28)
DEBUG Acquired lock for `E:\AppData\uv\python`
WARN Ignoring malformed managed Python entry:
    Failed to parse Python installation key `bin`: expected exactly 5 `-`-separated values, got 1
WARN Ignoring malformed managed Python entry:
    Failed to parse Python installation key `cache`: expected exactly 5 `-`-separated values, got 1
DEBUG No installation found for request `3.12 (cpython-3.12-windows-x86_64-none)`
DEBUG Found download `cpython-3.12.11-windows-x86_64-none` for request `3.12 (cpython-3.12-windows-x86_64-none)`
DEBUG Using request timeout of 30s
DEBUG Downloading https://github.com/astral-sh/python-build-standalone/releases/download/20250828/cpython-3.12.11%2B20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz to `E:\AppData\uv\python\cache\0b8fab064-cpython-3.12.11-20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz`
DEBUG Transient request failure for https://github.com/astral-sh/python-build-standalone/releases/download/20250828/cpython-3.12.11%2B20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz, retrying: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250828/cpython-3.12.11%2B20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz)
  Caused by: client error (Connect)
  Caused by: 远程主机强迫关闭了一个现有的连接。 (os error 10054)
DEBUG Transient request failure for https://github.com/astral-sh/python-build-standalone/releases/download/20250828/cpython-3.12.11%2B20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz, retrying: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250828/cpython-3.12.11%2B20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz)
  Caused by: client error (Connect)
  Caused by: 远程主机强迫关闭了一个现有的连接。 (os error 10054)
```



---

_Comment by @konstin on 2025-08-29 09:44_

Can you add the `0b8fab064-` prefix to the file? We need use that hash to ensure we have the right file.

---

_Comment by @classronin on 2025-08-29 09:46_

@konstin 在未来新的20250904/20250912/2...我怎么知道哈希值？ 

---

_Comment by @classronin on 2025-08-29 09:55_

@konstin 

```
E:\AppData\uv\python\cache>certutil -hashfile 0b8fab064-cpython-3.12.11-20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz SHA256
SHA256 的 0b8fab064-cpython-3.12.11-20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz 哈希:
0b8fab064f852d3ccb7cf30f58195452c1d70f494a556b4c003889aa2f93038f
CertUtil: -hashfile 命令成功完成。
```

是开头取9位的哈希值吧。


---

_Comment by @konstin on 2025-08-29 09:57_

The intended use for `UV_PYTHON_CACHE_DIR` is that you download Python archives with internet, and then can use the directory later without internet.

---

_Comment by @classronin on 2025-08-29 10:03_

@konstin 了解了，我是指’特意指定‘本地python包来安装，uv有这个方法吗？

---

_Comment by @konstin on 2025-08-29 10:09_

We currently don't have support for giving the command a local file to install.

---

_Comment by @FishAlchemist on 2025-08-30 04:51_

@classronin 
中文(Chinese)：
即使一个简单的命令无法解决这个问题，你也可以通过使用镜像源，将源地址指向一个本地位置，然后下载到 Python 中。
English:
Even though a simple command can't solve it. By using a mirror source, you can point the source to a local location and then download it to Python.

(The above text is identical in both Chinese and English.)
(以上内容的中英文是相同的。)

Example of command:
```
uv python install cpython-3.13.6 --mirror file://your_path
```
https://docs.astral.sh/uv/reference/environment/#uv_python_install_mirror

---

_Comment by @classronin on 2025-08-30 07:36_

@FishAlchemist 这才是货真价实的硬货！！！你是我心中的英雄。

```
>uv python install cpython-3.12.11 --mirror file:///E:/AppData/uv/python/cache/ -v
DEBUG uv 0.8.14 (af856fb88 2025-08-28)
DEBUG Acquired lock for `E:\AppData\uv\python`
WARN Ignoring malformed managed Python entry:
    Failed to parse Python installation key `bin`: expected exactly 5 `-`-separated values, got 1
WARN Ignoring malformed managed Python entry:
    Failed to parse Python installation key `cache`: expected exactly 5 `-`-separated values, got 1
DEBUG No installation found for request `cpython-3.12.11-any-any-any (cpython-3.12.11-windows-x86_64-none)`
DEBUG Found download `cpython-3.12.11-windows-x86_64-none` for request `cpython-3.12.11-any-any-any (cpython-3.12.11-windows-x86_64-none)`
DEBUG Using request timeout of 30s
DEBUG Downloading file:///E:/AppData/uv/python/cache/20250828/cpython-3.12.11%2B20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz to `E:\AppData\uv\python\cache\0b8fab064-cpython-3.12.11-20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz`
Downloading cpython-3.12.11-windows-x86_64-none (download) (20.2MiB)
 Downloading cpython-3.12.11-windows-x86_64-none (download)
DEBUG Extracting `E:\AppData\uv\python\cache\0b8fab064-cpython-3.12.11-20250828-x86_64-pc-windows-msvc-install_only_stripped.tar.gz`
Extracting cpython-3.12.11-windows-x86_64-none (extract) (20.2MiB)
 Extracting cpython-3.12.11-windows-x86_64-none (extract)
DEBUG Moving E:\AppData\uv\python\.temp\.tmpxMByZb\python to E:\AppData\uv\python\cpython-3.12.11-windows-x86_64-none
DEBUG Installed executable at `E:\AppData\uv\python\bin\python3.12.exe` for cpython-3.12.11-windows-x86_64-none
Installed Python 3.12.11 in 5.62s
 + cpython-3.12.11-windows-x86_64-none (python3.12.exe)
DEBUG Released lock at `E:\AppData\uv\python\.lock`
```




---

_Closed by @classronin on 2025-08-30 07:36_

---
