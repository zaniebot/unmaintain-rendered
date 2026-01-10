---
number: 14840
title: Installing python3 with an artifactory mirror fails on untarring files.
type: issue
state: closed
author: Ronaphil
labels:
  - bug
assignees: []
created_at: 2025-07-23T11:39:53Z
updated_at: 2025-07-23T12:39:32Z
url: https://github.com/astral-sh/uv/issues/14840
synced_at: 2026-01-10T01:25:49Z
---

# Installing python3 with an artifactory mirror fails on untarring files.

---

_Issue opened by @Ronaphil on 2025-07-23 11:39_

### Summary

As the title states I'm having some issues with using the UV_PYTHON_INSTALL_MIRROR env variable.
The files are present on the mirror and I can download and untar the file manually.
The same output is given when authenticated or unauthenticated.

As UV cleans up the downloaded files on failure I am unable to provide or debug the downloaded tar file.

Please see the logging output below. 

```bash
❯ export UV_PYTHON_INSTALL_MIRROR=https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download
❯ uv python install 3.13 -vvv 
DEBUG uv 0.8.2
TRACE Checking lock for `/home/user/.local/share/uv/python` at `/home/user/.local/share/uv/python/.lock`
DEBUG Acquired lock for `/home/user/.local/share/uv/python`
TRACE Found `ld` path: /lib64/ld-linux-x86-64.so.2
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help' for more information.\n"
TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`
DEBUG No installation found for request `Python 3.13`
DEBUG Found download `cpython-3.13.5-linux-x86_64-gnu` for request `Python 3.13`
DEBUG Using request timeout of 30s
DEBUG Downloading https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
DEBUG Extracting cpython-3.13.5-20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /home/user/.local/share/uv/python/.temp/.tmpfav5TZ
TRACE Handling request for https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz with authentication policy auto
TRACE Request for https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz is unauthenticated, checking cache
TRACE No credentials in cache for URL https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
TRACE Attempting unauthenticated request for https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
TRACE checkout waiting for idle connection: ("https", artifactory.example.com)
DEBUG starting new connection: https://artifactory.example.com/    
TRACE Http::connect; scheme=Some("https"), host=Some("artifactory.example.com"), port=None
DEBUG connecting to 172.21.10.52:443
DEBUG connected to 172.21.10.52:443
TRACE http1 handshake complete, spawning background dispatcher task
TRACE waiting for connection to be ready
TRACE connection is ready
TRACE checkout dropped for ("https", artifactory.example.com)
Downloading cpython-3.13.5-linux-x86_64-gnu (download)
TRACE Considering retry of error: ExtractError("cpython-3.13.5-20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz", Io(Custom { kind: InvalidData, error: "Invalid gzip header" }))
TRACE Retrying error: `invalid data`
DEBUG Transient failure while handling response for cpython-3.13.5-linux-x86_64-gnu; retrying...
TRACE put; add idle connection for ("https", artifactory.example.com)
DEBUG pooling idle connection for ("https", artifactory.example.com)
TRACE idle interval checking for expired
DEBUG Downloading https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
DEBUG Extracting cpython-3.13.5-20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /home/user/.local/share/uv/python/.temp/.tmpeZ7TJu
TRACE Handling request for https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz with authentication policy auto
TRACE Request for https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz is unauthenticated, checking cache
TRACE No credentials in cache for URL https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
TRACE Attempting unauthenticated request for https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
TRACE take? ("https", artifactory.example.com): expiration = Some(90s)
DEBUG reuse idle connection for ("https", artifactory.example.com)
Downloading cpython-3.13.5-linux-x86_64-gnu (download)
TRACE Considering retry of error: ExtractError("cpython-3.13.5-20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz", Io(Custom { kind: InvalidData, error: "Invalid gzip header" }))
TRACE Retrying error: `invalid data`
DEBUG Transient failure while handling response for cpython-3.13.5-linux-x86_64-gnu; retrying...
TRACE put; add idle connection for ("https", artifactory.example.com)
DEBUG pooling idle connection for ("https", artifactory.example.com)
DEBUG Downloading https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
DEBUG Extracting cpython-3.13.5-20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /home/user/.local/share/uv/python/.temp/.tmpJoPMDb
TRACE Handling request for https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz with authentication policy auto
TRACE Request for https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz is unauthenticated, checking cache
TRACE No credentials in cache for URL https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
TRACE Attempting unauthenticated request for https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
TRACE take? ("https", artifactory.example.com): expiration = Some(90s)
DEBUG reuse idle connection for ("https", artifactory.example.com)
Downloading cpython-3.13.5-linux-x86_64-gnu (download)
TRACE Considering retry of error: ExtractError("cpython-3.13.5-20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz", Io(Custom { kind: InvalidData, error: "Invalid gzip header" }))
TRACE Retrying error: `invalid data`
DEBUG Transient failure while handling response for cpython-3.13.5-linux-x86_64-gnu; retrying...
TRACE put; add idle connection for ("https", artifactory.example.com)
DEBUG pooling idle connection for ("https", artifactory.example.com)
DEBUG Downloading https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
DEBUG Extracting cpython-3.13.5-20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /home/user/.local/share/uv/python/.temp/.tmpk2rHCQ
TRACE Handling request for https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz with authentication policy auto
TRACE Request for https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz is unauthenticated, checking cache
TRACE No credentials in cache for URL https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
TRACE Attempting unauthenticated request for https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
TRACE take? ("https", artifactory.example.com): expiration = Some(90s)
DEBUG reuse idle connection for ("https", artifactory.example.com)
Downloading cpython-3.13.5-linux-x86_64-gnu (download)
TRACE Considering retry of error: ExtractError("cpython-3.13.5-20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz", Io(Custom { kind: InvalidData, error: "Invalid gzip header" }))
TRACE Retrying error: `invalid data`
error: Failed to install cpython-3.13.5-linux-x86_64-gnu
  Caused by: Request failed after 3 retries
  Caused by: Failed to extract archive: cpython-3.13.5-20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
  Caused by: I/O operation failed during extraction
  Caused by: Invalid gzip header
DEBUG Released lock at `/home/user/.local/share/uv/python/.lock`
````

The file downloaded manually from the same URL that UV uses seems fine:

```
❯ md5sum cpython-3.13.5+20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz 
76cf17ee964f2cdad9c950b4e39f115e  cpython-3.13.5+20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
````

### Platform

Linux 4.18.0-553.46.1.el8_10.x86_64 x86_64 GNU/Linux

### Version

uv 0.8.2

### Python version

Python 3.13.1

---

_Label `bug` added by @Ronaphil on 2025-07-23 11:39_

---

_Comment by @zanieb on 2025-07-23 12:11_

It's hard to say what the problem is since it's a third-party mirror. Does it work if you set `UV_PYTHON_CACHE_DIR=/tmp`?

---

_Comment by @Ronaphil on 2025-07-23 12:39_

That actually helped me tremendously @zanieb, thanks for the pointer.

Opening the text file revealed the following text, which is indeed not a valid gzip file 😄 
> We're sorry but jfrog webapp doesn't work properly without JavaScript enabled. Please enable it to continue.

For future reference the install mirror URL should be 

```bash
❯ export UV_PYTHON_INSTALL_MIRROR=https://artifactory.example.com/artifactory/github.com/astral-sh/python-build-standalone/releases/download/
```
instead of

```bash
❯  export UV_PYTHON_INSTALL_MIRROR=https://artifactory.example.com/ui/native/github.com/astral-sh/python-build-standalone/releases/download
```

---

_Closed by @Ronaphil on 2025-07-23 12:39_

---
