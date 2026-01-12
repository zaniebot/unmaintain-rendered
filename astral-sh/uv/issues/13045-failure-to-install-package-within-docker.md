```yaml
number: 13045
title: Failure to install package within docker container using uv while poetry/pip works fine
type: issue
state: closed
author: Skelmis
labels:
  - bug
assignees: []
created_at: 2025-04-22T08:06:25Z
updated_at: 2025-05-05T06:53:43Z
url: https://github.com/astral-sh/uv/issues/13045
synced_at: 2026-01-12T16:01:17Z
```

# Failure to install package within docker container using uv while poetry/pip works fine

---

_@Skelmis_

### Summary

When attempting to create a uv environment based on the uv docker images (Specifically `ghcr.io/astral-sh/uv:python3.10-alpine`), uv fails to install `semgrep` seemingly due to a lack of build wheels or source for the platform. This may have a wider impact, however this is the use case I've found it breaking in. I'd appreciate any help on the matter to get this resolved.

I have attempted to review the documentation, as well as other issues such as the issues related to adding pytorch from other indexes but have walked away without a solution. Other tools such as poetry and pip appear to work fine within the same environment.

Basic repro:
```dockerfile
FROM ghcr.io/astral-sh/uv:python3.10-alpine

WORKDIR /code
RUN uv init
RUN uv add semgrep
```

Which produces the following:
```text
docker build ./

[+] Building 3.9s (7/7) FINISHED                                                                                              docker:default
 => [internal] load build definition from Dockerfile                                                                                    0.0s
 => => transferring dockerfile: 126B                                                                                                    0.0s
 => [internal] load metadata for ghcr.io/astral-sh/uv:python3.10-alpine                                                                 0.8s
 => [internal] load .dockerignore                                                                                                       0.0s
 => => transferring context: 2B                                                                                                         0.0s
 => [1/4] FROM ghcr.io/astral-sh/uv:python3.10-alpine@sha256:b8a8a994c3b101b33d73f5a2aaf4ad555dc1d26cf538af5e7ded0e7eae832218           0.0s
 => CACHED [2/4] WORKDIR /code                                                                                                          0.0s
 => CACHED [3/4] RUN uv init                                                                                                            0.0s
 => ERROR [4/4] RUN uv add semgrep                                                                                                      3.0s
------                                                                                                                                       
 > [4/4] RUN uv add semgrep:                                                                                                                 
0.654 Using CPython 3.10.17 interpreter at: /usr/local/bin/python3.10                                                                        
0.654 Creating virtual environment at: .venv
2.861 Resolved 48 packages in 2.19s
2.877 error: Distribution `semgrep==1.119.0 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform
2.877 
2.877 hint: You're on Linux (`linux_x86_64`), but `semgrep` (v1.119.0) only has wheels for the following platforms: `manylinux2014_aarch64`, `manylinux2014_x86_64`, `musllinux_1_0_aarch64`, `musllinux_1_0_x86_64`, `macosx_10_14_x86_64`, `macosx_11_0_arm64`, `win_amd64`
------
Dockerfile:5
--------------------
   3 |     WORKDIR /code
   4 |     RUN uv init
   5 | >>> RUN uv add semgrep
--------------------
ERROR: failed to solve: process "/bin/sh -c uv add semgrep" did not complete successfully: exit code: 
```

However, modifiying the Dockerfile to the following works as expected:
```dockerfile
FROM ghcr.io/astral-sh/uv:python3.10-alpine

WORKDIR /code
RUN uv init
RUN uv pip install semgrep
```

With the verbose flag on the `uv add` command, the following output is received. As it is too long I have uploaded it to paste bin [here](https://pastebin.com/TEb581U0).

Similar errors are also raised when using other images such as `:alpine` or newer Python versions within the Python alpine images. 

### Platform

Linux 6.8.0-57-generic x86_64 Linux

### Version

uv 0.6.16

### Python version

Python 3.10.17

---

_Label `bug` added by @Skelmis on 2025-04-22 08:06_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-05-04 23:42_

---

_Comment by @charliermarsh on 2025-05-04 23:48_

I think the problem here is that they're faking the musllinux tag, and using a `musllinux_1_0`, but we treat `musllinux_1_1` as the lowest version in the spec:

https://github.com/semgrep/semgrep/blob/077885931f5dc296aa39cb609d9ef7ddd73d38de/cli/setup.py#L51-L55

I guess we can just lower the minimum to `musllinux_1_0` to match `packaging`:

https://github.com/pypa/packaging/blob/d0d5ad8687f666bea942d1ab4ee2feb5fa019d04/src/packaging/_musllinux.py#L71C1-L72C62

---

_Closed by @konstin on 2025-05-05 06:53_

---

_Closed by @konstin on 2025-05-05 06:53_

---
