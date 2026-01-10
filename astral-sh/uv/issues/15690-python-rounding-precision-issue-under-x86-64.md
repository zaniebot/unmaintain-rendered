```yaml
number: 15690
title: Python Rounding Precision Issue Under x86_64 Emulation
type: issue
state: closed
author: Bradleywboggs
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-09-04T22:18:49Z
updated_at: 2025-09-05T15:02:34Z
url: https://github.com/astral-sh/uv/issues/15690
synced_at: 2026-01-10T03:23:54Z
```

# Python Rounding Precision Issue Under x86_64 Emulation

---

_Issue opened by @Bradleywboggs on 2025-09-04 22:18_

### Summary

UV-installed Python versions exhibit critical rounding precision issues when running x86_64 containers under ARM64 emulation (Docker with Rosetta on Apple Silicon). The issue does not occur on native x86_64 or ARM64 systems.

**Key Finding**: `round(1.125, 2)` returns `1.0` instead of expected `1.12`

### Environment
- **Host**: Apple Silicon Mac (ARM64) + Docker Desktop + Rosetta 2
- **Affected**: x86_64 UV containers under emulation  
- **Unaffected**: Native x86_64/ARM64 systems, standard Python containers
- **Tested**: Python 3.8 through 3.14rc2

### Reproduction

**Quick test** (fails with UV managed python, passes with system Python):
```bash
#### UV managed Python - FAILS
docker run --platform linux/amd64 --rm ghcr.io/astral-sh/uv:python3.13-bookworm \
  uv run --managed-python python -c "print(round(1.125, 2))"
Downloading cpython-3.13.7-linux-x86_64-gnu (download) (30.8MiB)
 Downloading cpython-3.13.7-linux-x86_64-gnu (download)
1.0
```

```bash
#### System Python - WORKS  
docker run --platform linux/amd64 --rm ghcr.io/astral-sh/uv:python3.13-bookworm \
  python -c "print(round(1.125, 2))"
1.12
```

### Test Results

| Platform | Python Source | All Versions (3.8-3.14rc2) | `round(1.125, 2)` | Status |
|----------|---------------|----------------------------|-------------------|--------|
| x86_64 (emulated) | UV-installed | ❌ All affected | `1.0` | BROKEN |
| x86_64 (emulated) | Standard | ✅ All working | `1.12` | WORKS |
| x86_64 (native) | UV-installed | ✅ All working | `1.12` | WORKS |
| ARM64 (native) | UV-installed | ✅ All working | `1.12` | WORKS |

### Impact
- **Scope**: Development environments using x86_64 containers on Apple Silicon
- **Affected Operations**: Mathematical calculations, financial computations, any precision-dependent code
- **Production Risk**: None (issue limited to emulation environments)

### Workarounds
- Use ARM64-native UV containers on Apple Silicon
- Use standard Python images for x86_64 compatibility testing  
- Issue does not affect production deployments on native x86_64

### Analysis
Appears to be an emulation compatibility issue where UV's optimized x86_64 Python binaries don't interact correctly with ARM64-to-x86_64 emulation layers, specifically affecting floating-point operations.


### Platform
macOS 15.6: M1 Pro emulating x86_64

### Version

uv 0.8.15

### Python versions

3.8 - 3.14rc2

---

_Label `bug` added by @Bradleywboggs on 2025-09-04 22:18_

---

_Comment by @zanieb on 2025-09-04 22:56_

A couple things I'm confused by

1. `ghcr.io/astral-sh/uv:python3.9-bookworm` this image has Debian's Python pre-installed, not uv's
2. The reproduction you shared does not reproduce:


```
❯ docker run --platform linux/amd64 --rm ghcr.io/astral-sh/uv:python3.9-bookworm \
  python -c "print(round(1.125, 2))"
1.12
```

Even if you use uv's Python?

```
❯ docker run --platform linux/amd64 --rm ghcr.io/astral-sh/uv:python3.9-bookworm uv run --managed-python python -c "print(round(1.125, 2))"
Downloading cpython-3.13.7-linux-x86_64-gnu (download) (30.8MiB)
 Downloading cpython-3.13.7-linux-x86_64-gnu (download)
1.12
```


---

_Label `needs-mre` added by @zanieb on 2025-09-04 22:58_

---

_Comment by @zanieb on 2025-09-04 23:07_

For context, I'm on an M3 MacBook Pro

```
❯ uname -rsm
Darwin 23.6.0 arm64
❯ docker version
Client:
 Version:           27.5.1
 API version:       1.47
 Go version:        go1.22.11
 Git commit:        9f9e405
 Built:             Wed Jan 22 13:37:19 2025
 OS/Arch:           darwin/arm64
 Context:           orbstack
❯ orb version
Version: 1.10.3 (1100300)
Commit: 2b5dd5f580d80a3d2494b7b40dde2ef46813cfc5 (v1.10.3)
```

---

_Comment by @Bradleywboggs on 2025-09-04 23:52_

> For context, I'm on an M3 MacBook Pro
> 
> ```
> ❯ uname -rsm
> Darwin 23.6.0 arm64
> ❯ docker version
> Client:
>  Version:           27.5.1
>  API version:       1.47
>  Go version:        go1.22.11
>  Git commit:        9f9e405
>  Built:             Wed Jan 22 13:37:19 2025
>  OS/Arch:           darwin/arm64
>  Context:           orbstack
> ❯ orb version
> Version: 1.10.3 (1100300)
> Commit: 2b5dd5f580d80a3d2494b7b40dde2ef46813cfc5 (v1.10.3)
> ```

Interesting! I'll go back and investigate further and make updates to the issue or close it.

### UPDATE
Thanks for pointing out the discrepancy.
I've updated the steps for reproducing. I do indeed get the behavior only when running with the **uv managed** python installation.  That  got lost in my final edits of my notes.

```
λ>  uname -rsm
Darwin 24.6.0 arm64

λ> docker version
Client:
 Version:           27.5.1
 API version:       1.47
 Go version:        go1.22.11
 Git commit:        9f9e405
 Built:             Wed Jan 22 13:37:19 2025
 OS/Arch:           darwin/arm64
 Context:           desktop-linux

Server: Docker Desktop 4.38.0 (181591)
 Engine:
  Version:          27.5.1
  API version:      1.47 (minimum version 1.24)
  Go version:       go1.22.11
  Git commit:       4c9b3b0
  Built:            Wed Jan 22 13:41:25 2025
  OS/Arch:          linux/arm64
  Experimental:     false
 containerd:
  Version:          1.7.25
  GitCommit:        bcc810d6b9066471b0b6fa75f557a15a1cbf31bb
 runc:
  Version:          1.1.12
  GitCommit:        v1.1.12-0-g51d5e946
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

@zanieb So the main differerences in our setups are:
- Docker Desktop vs OrbStack
-  Darwin 24.6.0  vs Darwin 23.6.0 
- M1 Pro vs M3 chips (That shouldn't affect emulation, though. The Rosetta versions are the same, I think?)


---

_Comment by @konstin on 2025-09-05 07:07_

This looks like a bug in OrbStack or some other part of the emulation to me, have you tried the latest OrbStack version? If it works on the native platform, the uv-provided binary should be fine and the problem is in the emulation layer.

---

_Comment by @zsol on 2025-09-05 08:28_

This doesn't reproduce for me either (I'm not using rosetta for emulation):
```
❯ docker version
Client:
 Version:           28.3.3
 API version:       1.51
 Go version:        go1.24.5
 Git commit:        980b856
 Built:             Fri Jul 25 11:33:03 2025
 OS/Arch:           darwin/arm64
 Context:           desktop-linux

Server: Docker Desktop 4.45.0 (203075)
 Engine:
  Version:          28.3.3
  API version:      1.51 (minimum version 1.24)
  Go version:       go1.24.5
  Git commit:       bea959c
  Built:            Fri Jul 25 11:35:32 2025
  OS/Arch:          linux/arm64
  Experimental:     false
 containerd:
  Version:          1.7.27
  GitCommit:        05044ec0a9a75232cad458027ca83437aae3f4da
 runc:
  Version:          1.2.5
  GitCommit:        v1.2.5-0-g59923ef
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
❯ uname -rsm
Darwin 24.6.0 arm64
❯ docker run --platform linux/amd64 --rm ghcr.io/astral-sh/uv:python3.13-bookworm \
        uv run --managed-python python -c "print(round(1.125, 2))"
Downloading cpython-3.13.7-linux-x86_64-gnu (download) (30.8MiB)
 Downloading cpython-3.13.7-linux-x86_64-gnu (download)
1.12
```

---

_Comment by @Bradleywboggs on 2025-09-05 11:48_

> This looks like a bug in OrbStack or some other part of the emulation to me, have you tried the latest OrbStack version? If it works on the native platform, the uv-provided binary should be fine and the problem is in the emulation layer.

It's strange to me that the system provided Python consistently works when emulating, though. There seems to be something in how the uv managed binary is being compiled to trigger the issue (at least on my machine)

I'm not using orb stack. But I can give it a try.


---

_Comment by @zanieb on 2025-09-05 13:14_

> This looks like a bug in OrbStack 

I'm using OrbStack, they are using Docker Desktop.

---

_Comment by @konstin on 2025-09-05 13:19_

Sorry for mixing those up!

---

_Comment by @zanieb on 2025-09-05 13:21_

@zsol what's doing the x86-64 emulation in your setup?

---

_Comment by @szicari-streambit on 2025-09-05 14:13_

For what it's worth, I experience the same issue on an M1 Mac with vanilla Docker Desktop installed.

```shell
docker run --platform linux/amd64 --rm ghcr.io/astral-sh/uv:python3.13-bookworm \
 uv run --managed-python python -c "print(round(1.125, 2))"
Unable to find image 'ghcr.io/astral-sh/uv:python3.13-bookworm' locally
python3.13-bookworm: Pulling from astral-sh/uv
f014853ae203: Pull complete 
6d6401b7636b: Pull complete 
cffef7dc6f99: Pull complete 
1e6ffe3614ab: Pull complete 
bcfb14ce47c8: Pull complete 
b390d6738a63: Pull complete 
1fe8038e5470: Pull complete 
d88ad2a93a61: Pull complete 
Digest: sha256:d939988134ca1d678d2b1da32862e9ea206e9316fa77697f872b1c42fae61e7a
Status: Downloaded newer image for ghcr.io/astral-sh/uv:python3.13-bookworm
Downloading cpython-3.13.7-linux-x86_64-gnu (download) (30.8MiB)
 Downloading cpython-3.13.7-linux-x86_64-gnu (download)
1.0
```

---

_Comment by @szicari-streambit on 2025-09-05 14:46_

Further research yielded that switching from the Apple virtualization framework to Qemu resulted in "correct" behavior.

```shell
docker run --platform linux/amd64 --rm ghcr.io/astral-sh/uv:python3.13-bookworm \
 uv run --managed-python python -c "print(round(1.125, 2))"
Downloading cpython-3.13.7-linux-x86_64-gnu (download) (30.8MiB)
 Downloading cpython-3.13.7-linux-x86_64-gnu (download)
1.12
```

---

_Comment by @Bradleywboggs on 2025-09-05 14:51_

Also, simply disabling Rosetta produced correct results, too.

<img width="802" height="400" alt="Image" src="https://github.com/user-attachments/assets/559e6f06-7c0f-4da0-a497-0c8990b27f0e" />

```bash
λ> docker run --platform linux/amd64 --rm ghcr.io/astral-sh/uv:python3.13-bookworm \
 uv run --managed-python python -c "print(round(1.125, 2))"
Downloading cpython-3.13.7-linux-x86_64-gnu (download) (30.8MiB)
 Downloading cpython-3.13.7-linux-x86_64-gnu (download)
1.12
```

---

_Comment by @Bradleywboggs on 2025-09-05 14:55_

It appears this is a problem with Rosetta in other contexts as well:
[https://github.com/oven-sh/bun/issues/19677](https://github.com/oven-sh/bun/issues/19677)

I'm going to close the issue, since it doesn't appear to be a `uv` problem.


---

_Closed by @Bradleywboggs on 2025-09-05 14:55_

---

_Comment by @zanieb on 2025-09-05 15:02_

For future reference, it looks like Apple has fixed this per https://github.com/oven-sh/bun/issues/19677#issuecomment-3114724055 but it may not have released yet.

---
