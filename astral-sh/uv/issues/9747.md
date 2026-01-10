```yaml
number: 9747
title: "Downloading python from behind corporate proxy fails to download in `uv` but works with `wget` and `rye`"
type: issue
state: closed
author: aglove2189
labels:
  - bug
  - network
assignees: []
created_at: 2024-12-09T19:39:28Z
updated_at: 2025-03-13T11:02:52Z
url: https://github.com/astral-sh/uv/issues/9747
synced_at: 2026-01-10T03:50:30Z
```

# Downloading python from behind corporate proxy fails to download in `uv` but works with `wget` and `rye`

---

_Issue opened by @aglove2189 on 2024-12-09 19:39_

## uv
```sh
RUST_LOG=uv=trace uv --native-tls python install 3.11 --no-cache
```
```sh
DEBUG uv 0.5.4
TRACE ld path: /lib64/ld-linux-x86-64.so.2
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help' for more information.\n"
TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`
TRACE Tried to find libc version from possible symlink at "/lib64/ld-linux-x86-64.so.2", but failed: Failed to find glibc version in the filename of linker: `/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2`
TRACE stdout output from `ldd --version`: "ld.so (Debian GLIBC 2.36-9+deb12u8) stable release version 2.36.\nCopyright (C) 2022 Free Software Foundation, Inc.\nThis is free software; see the source for copying conditions.\nThere is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A\nPARTICULAR PURPOSE.\n"
TRACE Found manylinux 2.36 in stdout of `ldd --version`
TRACE Checking lock for `/home/akglover/.local/share/uv/python` at `/home/akglover/.local/share/uv/python/.lock`
DEBUG Acquired lock for `/home/akglover/.local/share/uv/python`
TRACE Found existing installation cpython-3.12.6-linux-x86_64-gnu
TRACE Found existing installation cpython-3.9.20-linux-x86_64-gnu
DEBUG No installation found for request `Python 3.11`
DEBUG Found download `cpython-3.11.10-linux-x86_64-gnu` for request `Python 3.11`
DEBUG Using request timeout of 30s
TRACE Handling request for https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
TRACE Request for https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz is unauthenticated, checking cache
TRACE No credentials in cache for URL https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
TRACE Attempting unauthenticated request for https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
cpython-3.11.10-linux-x86_64-gnu ------------------------------     0 B/20.22 MiB                                                       DEBUG Downloading https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /home/akglover/.local/share/uv/python/.cache/.tmpWo6eia
DEBUG Extracting cpython-3.11.10%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
cpython-3.11.10-linux-x86_64-gnu ------------------------------ 12.20 MiB/20.22 MiB                                                     TRACE Attempting to retry error: ExtractError("cpython-3.11.10%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz", Io(Custom { kind: UnexpectedEof, error: TarError { desc: "failed to unpack `/home/akglover/.local/share/uv/python/.cache/.tmpWo6eia/python/lib/python3.11/ensurepip/_bundled/pip-24.0-py3-none-any.whl`", io: Custom { kind: UnexpectedEof, error: TarError { desc: "failed to unpack `python/lib/python3.11/ensurepip/_bundled/pip-24.0-py3-none-any.whl` into `/home/akglover/.local/share/uv/python/.cache/.tmpWo6eia/python/lib/python3.11/ensurepip/_bundled/pip-24.0-py3-none-any.whl`", io: Custom { kind: UnexpectedEof, error: "unexpected end of file" } } } } }))
TRACE Cannot retry error: not a reqwest error
error: Failed to install cpython-3.11.10-linux-x86_64-gnu
  Caused by: Failed to extract archive: cpython-3.11.10%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
  Caused by: failed to unpack `/home/akglover/.local/share/uv/python/.cache/.tmpWo6eia/python/lib/python3.11/ensurepip/_bundled/pip-24.0-py3-none-any.whl`
  Caused by: failed to unpack `python/lib/python3.11/ensurepip/_bundled/pip-24.0-py3-none-any.whl` into `/home/akglover/.local/share/uv/python/.cache/.tmpWo6eia/python/lib/python3.11/ensurepip/_bundled/pip-24.0-py3-none-any.whl`
  Caused by: unexpected end of file
DEBUG Released lock at `/home/akglover/.local/share/uv/python/.lock`
```

## rye
```sh
rye toolchain fetch cpython@3.11 -v
```
```sh
target dir: /home/akglover/.rye/py/cpython@3.11.10
download url: https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-x86_64-unknown-linux-gnu-pgo%2Blto-full.tar.zst
Downloading cpython@3.11.10
Checking checksum
Unpacking
Downloaded cpython@3.11.10
```

## wget
```sh
wget https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
```

```sh
--2024-12-09 13:28:29--  https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
Resolving github.com (github.com)... 140.82.113.4
Connecting to github.com (github.com)|140.82.113.4|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/162334160/bc9e9129-99c7-4349-b9d9-0d423765697a?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=releaseassetproduction%2F20241209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20241209T192638Z&X-Amz-Expires=300&X-Amz-Signature=d217e3df5b2776df29cc2c970250f40489934119c2fc641228a91937cd953d88&X-Amz-SignedHeaders=host&response-content-disposition=attachment%3B%20filename%3Dcpython-3.11.10%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz&response-content-type=application%2Foctet-stream [following]
--2024-12-09 13:28:29--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/162334160/bc9e9129-99c7-4349-b9d9-0d423765697a?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=releaseassetproduction%2F20241209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20241209T192638Z&X-Amz-Expires=300&X-Amz-Signature=d217e3df5b2776df29cc2c970250f40489934119c2fc641228a91937cd953d88&X-Amz-SignedHeaders=host&response-content-disposition=attachment%3B%20filename%3Dcpython-3.11.10%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.111.133, 185.199.110.133, 185.199.109.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.111.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 21205300 (20M) [application/octet-stream]
Saving to: ‘cpython-3.11.10+20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz.1’

cpython-3.11.10+20241016-x86_64-u  60%[====================================>                         ]  12.28M  1.52MB/s    in 8.1s    

2024-12-09 13:28:37 (1.52 MB/s) - Read error at byte 12876058/21205300 (Error in the pull function.). Retrying.

--2024-12-09 13:28:38--  (try: 2)  https://objects.githubusercontent.com/github-production-release-asset-2e65be/162334160/bc9e9129-99c7-4349-b9d9-0d423765697a?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=releaseassetproduction%2F20241209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20241209T192638Z&X-Amz-Expires=300&X-Amz-Signature=d217e3df5b2776df29cc2c970250f40489934119c2fc641228a91937cd953d88&X-Amz-SignedHeaders=host&response-content-disposition=attachment%3B%20filename%3Dcpython-3.11.10%2B20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz&response-content-type=application%2Foctet-stream
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.111.133|:443... connected.
HTTP request sent, awaiting response... 206 Partial Content
Length: 21205300 (20M), 8329242 (7.9M) remaining [application/octet-stream]
Saving to: ‘cpython-3.11.10+20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz.1’

cpython-3.11.10+20241016-x86_64-u 100%[+++++++++++++++++++++++++++++++++++++========================>]  20.22M  19.1MB/s    in 0.4s    

2024-12-09 13:28:39 (19.1 MB/s) - ‘cpython-3.11.10+20241016-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz.1’ saved [21205300/21205300]
```

---

_Comment by @aglove2189 on 2024-12-09 19:42_

Just a guess but it seems like the read gets corrupted, `wget` and/or `rye` retries / resumes but `uv` doesn't?

---

_Comment by @charliermarsh on 2024-12-09 19:49_

Are you seeing this consistently? Like every time?

---

_Comment by @charliermarsh on 2024-12-09 19:53_

I'm a little surprised we aren't retrying there... we do have retries setup, but I guess something is being lost because it's technically an extraction error? I'd have to find a way to reproduce it.

---

_Label `network` added by @charliermarsh on 2024-12-09 19:53_

---

_Comment by @aglove2189 on 2024-12-09 20:31_

Yes it's happening consistently across multiple machines and OSs

---

_Comment by @charliermarsh on 2024-12-09 20:35_

I'll look into it, though it seems not-great that your proxy is consistently failing :)

---

_Comment by @aglove2189 on 2024-12-09 20:38_

Oh trust me I very much agree :). I'm working internally on diagnosing the root proxy issue so no worries if you want to close this issue.

---

_Comment by @charliermarsh on 2024-12-09 20:40_

Na all good, we should be retrying here! I'm just worried that I'll have trouble emulating the exact error.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-09 21:59_

---

_Label `bug` added by @charliermarsh on 2024-12-09 21:59_

---

_Closed by @charliermarsh on 2024-12-10 12:33_

---
