```yaml
number: 11961
title: "`RUN sh /uv-installer.sh` raises error when following example in docs?"
type: issue
state: open
author: philiporlando
labels:
  - question
assignees: []
created_at: 2025-03-04T22:30:46Z
updated_at: 2025-03-04T23:29:20Z
url: https://github.com/astral-sh/uv/issues/11961
synced_at: 2026-01-12T16:00:50Z
```

# `RUN sh /uv-installer.sh` raises error when following example in docs?

---

_@philiporlando_

### Summary

I'm seeing the below error while following along with the [Using uv in Docker](https://docs.astral.sh/uv/guides/integration/docker/#installing-uv) guide. Here is my Dockerfile:

```Dockerfile
FROM python:3.12.2-slim-bookworm

# The installer requires curl (and certificates) to download the release archive
RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates

# Download the latest installer
ADD https://astral.sh/uv/0.6.4/install.sh /uv-installer.sh

# Run the installer then remove it
RUN sh /uv-installer.sh && rm /uv-installer.sh

# Ensure the installed binary is on the `PATH`
ENV PATH="/root/.local/bin/:$PATH"
```

```
docker build -t test_build .
[+] Building 9.6s (8/8) FINISHED                                                                                                               docker:default
 => [internal] load .dockerignore                                                                                                                        0.0s
 => => transferring context: 45B                                                                                                                         0.0s 
 => [internal] load build definition from Dockerfile                                                                                                     0.0s 
 => => transferring dockerfile: 503B                                                                                                                     0.0s 
 => [internal] load metadata for docker.io/library/python:3.12.2-slim-bookworm                                                                           1.1s 
 => [1/4] FROM docker.io/library/python:3.12.2-slim-bookworm@sha256:5dc6f84b5e97bfb0c90abfb7c55f3cacc2cb6687c8f920b64a833a2219875997                     3.5s
 => => resolve docker.io/library/python:3.12.2-slim-bookworm@sha256:5dc6f84b5e97bfb0c90abfb7c55f3cacc2cb6687c8f920b64a833a2219875997                     0.0s 
 => => sha256:5dc6f84b5e97bfb0c90abfb7c55f3cacc2cb6687c8f920b64a833a2219875997 1.65kB / 1.65kB                                                           0.0s 
 => => sha256:ac212230555ffb7ec17c214fb4cf036ced11b30b5b460994376b0725c7f6c151 1.37kB / 1.37kB                                                           0.0s
 => => sha256:23cd4350f4bde96406bb9025059e69cb9d533a372427877ec86d2a38083eb02d 6.71kB / 6.71kB                                                           0.0s 
 => => sha256:8a1e25ce7c4f75e372e9884f8f7b1bedcfe4a7a7d452eb4b0a1c7477c9a90345 29.12MB / 29.12MB                                                         1.4s 
 => => sha256:1103112ebfc46e01c0f35f3586e5a39c6a9ffa32c1a362d4d5f20e3783c6fdd7 3.51MB / 3.51MB                                                           0.3s 
 => => sha256:e71929d491677492d7a4ad57a7fa341553306ea7568c09e4231dcc509dd00ae9 11.99MB / 11.99MB                                                         0.8s 
 => => sha256:c529235f83c86ca0fb587777a0c0180058e05e0b37f760212ff224a20d83a9b8 245B / 245B                                                               0.5s
 => => sha256:a47354887c310a4b86f2eec46704051aa91513f71ef42a44dcedeea84062c163 2.98MB / 2.98MB                                                           0.8s
 => => extracting sha256:8a1e25ce7c4f75e372e9884f8f7b1bedcfe4a7a7d452eb4b0a1c7477c9a90345                                                                1.2s
 => => extracting sha256:1103112ebfc46e01c0f35f3586e5a39c6a9ffa32c1a362d4d5f20e3783c6fdd7                                                                0.1s
 => => extracting sha256:e71929d491677492d7a4ad57a7fa341553306ea7568c09e4231dcc509dd00ae9                                                                0.4s
 => => extracting sha256:c529235f83c86ca0fb587777a0c0180058e05e0b37f760212ff224a20d83a9b8                                                                0.0s
 => => extracting sha256:a47354887c310a4b86f2eec46704051aa91513f71ef42a44dcedeea84062c163                                                                0.2s
 => https://astral.sh/uv/install.sh                                                                                                                      0.1s 
 => [2/4] RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates                                                          4.4s 
 => [3/4] ADD https://astral.sh/uv/install.sh /uv-installer.sh                                                                                           0.0s 
 => ERROR [4/4] RUN sh /uv-installer.sh && rm /uv-installer.sh                                                                                           0.4s 
------
 > [4/4] RUN sh /uv-installer.sh && rm /uv-installer.sh:
0.358 /uv-installer.sh: 1: cannot open html: No such file
0.358 /uv-installer.sh: 2: cannot open head: No such file
0.358 /uv-installer.sh: 3: cannot open meta: No such file
0.358 /uv-installer.sh: 4: cannot open META: No such file
0.358 /uv-installer.sh: 5: cannot open title: No such file
0.358 /uv-installer.sh: 6: cannot open style: No such file
0.358 /uv-installer.sh: 7: form: not found
0.358 /uv-installer.sh: 8: font-size:: not found
: not foundnstaller.sh: 8:
0.358 /uv-installer.sh: 9: font-weight:: not found
: not foundnstaller.sh: 9:
0.358 /uv-installer.sh: 10: width:: not found
: not foundnstaller.sh: 10:
0.358 /uv-installer.sh: 11: border-radius:: not found
: not foundnstaller.sh: 11:
0.358 /uv-installer.sh: 12: background-color:: not found
0.358 /uv-installer.sh: 13: color:: not found
0.359 /uv-installer.sh: 14: border:: not found
: not foundnstaller.sh: 15: }
0.359 /uv-installer.sh: 17: vertical-align:: not found
: not foundnstaller.sh: 17:
0.359 /uv-installer.sh: 18: text-align:: not found
: not foundnstaller.sh: 18:
: not foundnstaller.sh: 19: }
0.359 /uv-installer.sh: 21: margin-left:: not found
: not foundnstaller.sh: 21:
0.359 /uv-installer.sh: 22: margin-right:: not found
: not foundnstaller.sh: 22:
0.359 /uv-installer.sh: 23: margin-top:: not found
: not foundnstaller.sh: 23:
0.359 /uv-installer.sh: 24: margin-bottom:: not found
: not foundnstaller.sh: 24:
0.359 /uv-installer.sh: 25: vertical-align:: not found
: not foundnstaller.sh: 25:
0.359 /uv-installer.sh: 26: text-align:: not found
: not foundnstaller.sh: 26:
0.359 /uv-installer.sh: 27: padding-top:: not found
: not foundnstaller.sh: 27:
0.359 /uv-installer.sh: 28: font-weight:: not found
: not foundnstaller.sh: 28:
: not foundnstaller.sh: 29: }
0.359 /uv-installer.sh: 30: cannot open /style: No such file
0.359 /uv-installer.sh: 31: cannot open script: No such file
0.359 /uv-installer.sh: 32: Syntax error: "(" unexpected
------
Dockerfile:10
--------------------
   8 |
   9 |     # Run the installer then remove it
  10 | >>> RUN sh /uv-installer.sh && rm /uv-installer.sh
  11 |
  12 |     # Ensure the installed binary is on the `PATH`
--------------------
ERROR: failed to solve: process "/bin/sh -c sh /uv-installer.sh && rm /uv-installer.sh" did not complete successfully: exit code: 2
```

### Platform

Windows 10 x86_64

### Version

0.6.4

### Python version

Python 3.12.2

---

_Label `bug` added by @philiporlando on 2025-03-04 22:30_

---

_Comment by @zanieb on 2025-03-04 22:34_

Hm, I'm not sure what's up with that cc @Gankra 

I'd strongly recommend just using the `COPY` install instructions

```
COPY --from=ghcr.io/astral-sh/uv:0.6.4 /uv /uvx /bin/
```

It's faster and simpler.

---

_Comment by @philiporlando on 2025-03-04 22:38_

Thanks for the suggestion. This approach is working for me!

```Dockerfile
FROM python:3.12.2-slim-bookworm
COPY --from=ghcr.io/astral-sh/uv:0.6.4 /uv /uvx /bin/
```

```
docker build -t test_build .
[+] Building 2.7s (8/8) FINISHED                                                                                                               docker:default
 => [internal] load .dockerignore                                                                                                                        0.0s
 => => transferring context: 45B                                                                                                                         0.0s 
 => [internal] load build definition from Dockerfile                                                                                                     0.0s 
 => => transferring dockerfile: 124B                                                                                                                     0.0s 
 => [internal] load metadata for ghcr.io/astral-sh/uv:0.6.4                                                                                              1.4s 
 => [internal] load metadata for docker.io/library/python:3.12.2-slim-bookworm                                                                           1.4s 
 => FROM ghcr.io/astral-sh/uv:0.6.4@sha256:0d686193e6d06a262184e4367d00276e24a524357080868c1732c2718f75d4d9                                              0.9s
 => => resolve ghcr.io/astral-sh/uv:0.6.4@sha256:0d686193e6d06a262184e4367d00276e24a524357080868c1732c2718f75d4d9                                        0.0s 
 => => sha256:0d686193e6d06a262184e4367d00276e24a524357080868c1732c2718f75d4d9 2.19kB / 2.19kB                                                           0.0s 
 => => sha256:92b8943f7334b685e5cdfc2f88363528645ca994041e7957ac11884fc4f6e4cb 669B / 669B                                                               0.0s 
 => => sha256:df01d1e7b8da8f27df13f15bb76e9aef1df01822241f0f1a58ee704b46721a3e 1.30kB / 1.30kB                                                           0.0s
 => => sha256:05ccfcefafe5434c1460cdeea9443c4eb6b11f3e24345c40d4313f0779a73906 16.14MB / 16.14MB                                                         0.6s 
 => => sha256:6949b2fc5cd6f9b4a6604285d16c7c0f6230be103936d24f6dac1f506007ba1f 94B / 94B                                                                 0.2s 
 => => extracting sha256:05ccfcefafe5434c1460cdeea9443c4eb6b11f3e24345c40d4313f0779a73906                                                                0.2s 
 => => extracting sha256:6949b2fc5cd6f9b4a6604285d16c7c0f6230be103936d24f6dac1f506007ba1f                                                                0.0s 
 => [stage-0 1/2] FROM docker.io/library/python:3.12.2-slim-bookworm@sha256:5dc6f84b5e97bfb0c90abfb7c55f3cacc2cb6687c8f920b64a833a2219875997             0.1s 
 => => resolve docker.io/library/python:3.12.2-slim-bookworm@sha256:5dc6f84b5e97bfb0c90abfb7c55f3cacc2cb6687c8f920b64a833a2219875997                     0.0s 
 => => sha256:5dc6f84b5e97bfb0c90abfb7c55f3cacc2cb6687c8f920b64a833a2219875997 1.65kB / 1.65kB                                                           0.0s 
 => => sha256:ac212230555ffb7ec17c214fb4cf036ced11b30b5b460994376b0725c7f6c151 1.37kB / 1.37kB                                                           0.0s
 => => sha256:23cd4350f4bde96406bb9025059e69cb9d533a372427877ec86d2a38083eb02d 6.71kB / 6.71kB                                                           0.0s
 => [stage-0 2/2] COPY --from=ghcr.io/astral-sh/uv:0.6.4 /uv /uvx /bin/                                                                                  0.1s 
 => exporting to image                                                                                                                                   0.1s 
 => => exporting layers                                                                                                                                  0.1s 
 => => writing image sha256:caf6044d0b23d4abb607ebcdf83ea417658b86c51de4293660c1c1f3e75f932f                                                             0.0s 
 => => naming to docker.io/library/test_build                                                                                                            0.0s 

What's Next?
  View a summary of image vulnerabilities and recommendations → docker scout quickview
```

---

_Comment by @zanieb on 2025-03-04 22:41_

For what it's worth, it looks like `ADD https://astral.sh/uv/install.sh` is not getting you the installer? That looks like an HTML payload. 

---

_Comment by @zanieb on 2025-03-04 22:43_

What does

```
ADD https://astral.sh/uv/install.sh /installer.sh
RUN cat /installer.sh && exit 1
```

or

```
ADD https://astral.sh/uv/0.6.4/install.sh /installer.sh
RUN cat /installer.sh && exit 1
```

show?

---

_Comment by @philiporlando on 2025-03-04 22:49_

It might have something to do with the corporate firewall I'm stuck behind? This is what I see when navigating to [astral.sh](https://astral.sh/) via web browser:

![Image](https://github.com/user-attachments/assets/5c1f2d9c-104e-42bc-bed4-3057bc1f3821)

I think IT is afraid of that scary `.sh` domain...

----

**Edit:** It seems like it's working if I'm not pinning the version?

```Dockerfile
ADD https://astral.sh/uv/install.sh /installer.sh
RUN cat /installer.sh && exit 1
```
```
0.193             if ! check_cmd b2sum; then
0.193                 say "skipping blake2b checksum verification (it requires the 'b2sum' command)"
0.193                 return 0
0.193             fi
0.193             _calculated_checksum="$(b2sum "$_file" | awk '{printf $1}')"
0.193             ;;
0.193         false)
0.193             ;;
0.193         *)
0.193             say "skipping unknown checksum style: $_checksum_style"
0.193             return 0
0.193             ;;
0.193     esac
0.193
0.193     if [ "$_calculated_checksum" != "$_checksum_value" ]; then
0.193         err "checksum mismatch
0.193             want: $_checksum_value
0.193             got:  $_calculated_checksum"
0.193     fi
0.193 }
0.193
0.193 download_binary_and_run_installer "$@" || exit 1
------
Dockerfile:8
--------------------
   6 |     # Download the latest installer
   7 |     ADD https://astral.sh/uv/install.sh /installer.sh
   8 | >>> RUN cat /installer.sh && exit 1
   9 |
  10 |     # RUN sh /uv-installer.sh && rm /uv-installer.sh
--------------------
ERROR: failed to solve: process "/bin/sh -c cat /installer.sh && exit 1" did not complete successfully: exit code: 1
```

----

But it looks like `installer.sh` isn't being downloaded at all when pinning the version?

```Dockerfile
ADD https://astral.sh/uv/0.6.4/install.sh /installer.sh
RUN cat /installer.sh && exit 1
```

```
 => ERROR [4/4] RUN cat /installer.sh && exit 1                                                                                                          0.2s 
------
 > [4/4] RUN cat /installer.sh && exit 1:
0.208 cat: /installer.sh: No such file or directory
------
Dockerfile:9
--------------------
   7 |     ADD https://astral.sh/uv/0.6.4/install.sh /uv-installer.sh
   8 |
   9 | >>> RUN cat /installer.sh && exit 1
  10 |
  11 |     # RUN sh /uv-installer.sh && rm /uv-installer.sh
--------------------
ERROR: failed to solve: process "/bin/sh -c cat /installer.sh && exit 1" did not complete successfully: exit code: 1
```

---

_Comment by @zanieb on 2025-03-04 22:58_

Ah lol that's silly. Not much we can do about that :) Thanks for checking.

---

_Label `bug` removed by @zanieb on 2025-03-04 22:58_

---

_Label `question` added by @zanieb on 2025-03-04 22:58_

---

_Comment by @philiporlando on 2025-03-04 23:00_

I don't think it's a firewall issue after all. It looks like I'm able to use the installer if I don't pin the version:

```Dockerfile
FROM python:3.12.2-slim-bookworm

# The installer requires curl (and certificates) to download the release archive
RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates

# Download the latest installer
ADD https://astral.sh/uv/install.sh /uv-installer.sh

# Run the installer then remove it
RUN sh /uv-installer.sh && rm /uv-installer.sh

# Ensure the installed binary is on the `PATH`
ENV PATH="/root/.local/bin/:$PATH"
```

```
docker build -t test_build .
[+] Building 8.6s (10/10) FINISHED                                                                                                             docker:default 
 => [internal] load build definition from Dockerfile                                                                                                     0.0s 
 => => transferring dockerfile: 503B                                                                                                                     0.0s 
 => [internal] load .dockerignore                                                                                                                        0.0s 
 => => transferring context: 45B                                                                                                                         0.0s 
 => [internal] load metadata for docker.io/library/python:3.12.2-slim-bookworm                                                                           1.6s 
 => [auth] library/python:pull token for registry-1.docker.io                                                                                            0.0s 
 => [1/4] FROM docker.io/library/python:3.12.2-slim-bookworm@sha256:5dc6f84b5e97bfb0c90abfb7c55f3cacc2cb6687c8f920b64a833a2219875997                     0.1s 
 => => resolve docker.io/library/python:3.12.2-slim-bookworm@sha256:5dc6f84b5e97bfb0c90abfb7c55f3cacc2cb6687c8f920b64a833a2219875997                     0.0s 
 => => sha256:5dc6f84b5e97bfb0c90abfb7c55f3cacc2cb6687c8f920b64a833a2219875997 1.65kB / 1.65kB                                                           0.0s 
 => => sha256:ac212230555ffb7ec17c214fb4cf036ced11b30b5b460994376b0725c7f6c151 1.37kB / 1.37kB                                                           0.0s 
 => => sha256:23cd4350f4bde96406bb9025059e69cb9d533a372427877ec86d2a38083eb02d 6.71kB / 6.71kB                                                           0.0s 
 => https://astral.sh/uv/install.sh                                                                                                                      0.7s 
 => [2/4] RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates                                                          5.1s 
 => [3/4] ADD https://astral.sh/uv/install.sh /uv-installer.sh                                                                                           0.0s 
 => [4/4] RUN sh /uv-installer.sh && rm /uv-installer.sh                                                                                                 1.6s 
 => exporting to image                                                                                                                                   0.1s 
 => => exporting layers                                                                                                                                  0.1s 
 => => writing image sha256:ef57cc97a8d93f6b73575359d40f8bf66b717b7d9357fca84f9c79b5f4ad82b5                                                             0.0s
 => => naming to docker.io/library/test_build                                                                                                            0.0s

What's Next?
  View a summary of image vulnerabilities and recommendations → docker scout quickview
```

If I then rebuild the Dockerfile after pinning the version, it will also work. I think caching is coming into play here?

```Dockerfile
FROM python:3.12.2-slim-bookworm

# The installer requires curl (and certificates) to download the release archive
RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates

# Download the latest installer
ADD https://astral.sh/uv/0.6.4/install.sh /uv-installer.sh

# Run the installer then remove it
RUN sh /uv-installer.sh && rm /uv-installer.sh

# Ensure the installed binary is on the `PATH`
ENV PATH="/root/.local/bin/:$PATH"
```

```
docker build -t test_build .
[+] Building 1.3s (9/9) FINISHED                                                                                                               docker:default
 => [internal] load .dockerignore                                                                                                                        0.0s
 => => transferring context: 45B                                                                                                                         0.0s 
 => [internal] load build definition from Dockerfile                                                                                                     0.0s 
 => => transferring dockerfile: 509B                                                                                                                     0.0s 
 => [internal] load metadata for docker.io/library/python:3.12.2-slim-bookworm                                                                           0.7s 
 => [1/4] FROM docker.io/library/python:3.12.2-slim-bookworm@sha256:5dc6f84b5e97bfb0c90abfb7c55f3cacc2cb6687c8f920b64a833a2219875997                     0.0s
 => https://astral.sh/uv/0.6.4/install.sh                                                                                                                0.6s 
 => CACHED [2/4] RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates                                                   0.0s
 => CACHED [3/4] ADD https://astral.sh/uv/0.6.4/install.sh /uv-installer.sh                                                                              0.0s 
 => CACHED [4/4] RUN sh /uv-installer.sh && rm /uv-installer.sh                                                                                          0.0s 
 => exporting to image                                                                                                                                   0.0s 
 => => exporting layers                                                                                                                                  0.0s 
 => => writing image sha256:f69e55b3bf8452ea42770a9aabf8694fd8c579536ad62dfb29df9633a76ab3ba                                                             0.0s 
 => => naming to docker.io/library/test_build                                                                                                            0.0s 

What's Next?
  View a summary of image vulnerabilities and recommendations → docker scout quickview
```

---
