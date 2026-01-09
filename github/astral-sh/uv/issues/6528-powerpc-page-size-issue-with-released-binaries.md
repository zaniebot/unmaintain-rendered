---
number: 6528
title: PowerPC page size issue with released binaries
type: issue
state: closed
author: rjzak
labels:
  - bug
  - help wanted
  - releases
assignees: []
created_at: 2024-08-23T15:47:33Z
updated_at: 2024-09-11T18:13:51Z
url: https://github.com/astral-sh/uv/issues/6528
synced_at: 2026-01-07T13:12:17-06:00
---

# PowerPC page size issue with released binaries

---

_Issue opened by @rjzak on 2024-08-23 15:47_

The released binaries for powerpc64le don't run on 64k page size systems, but uv runs on those systems when compiled from source.

```
~/Downloads
❯ ./uv-powerpc64le-unknown-linux-gnu/uv -V
<jemalloc>: Unsupported system page size
<jemalloc>: Unsupported system page size
memory allocation of 56 bytes failed
Aborted

~/Downloads
❯ ./uv-powerpc64le-unknown-linux-musl/uv -V
<jemalloc>: Unsupported system page size
<jemalloc>: Unsupported system page size
memory allocation of 56 bytes failed
Aborted

~/Downloads
❯ ./uv-0.3.2/target/release/uv -V
uv 0.3.2

~/Downloads
❯ uname -a
Linux behemoth 6.1.0-23-powerpc64le #1 SMP Debian 6.1.99-1 (2024-07-15) ppc64le GNU/Linux

~/Downloads
❯ lsb_release -a
No LSB modules are available.
Distributor ID:	Debian
Description:	Debian GNU/Linux 12 (bookworm)
Release:	12
Codename:	bookworm

~/Downloads
❯ getconf PAGESIZE
65536
```

This may be an issue since I think most ppc64 systems use 64k as that seems to be the default. As far as I know, the discontinued Void Linux PPC and Chimera Linux are the only ppc64 systems which used 4k pages.


---

_Comment by @zanieb on 2024-08-23 15:52_

Is this a duplicate of https://github.com/astral-sh/uv/issues/4647 but for our distributions?

---

_Comment by @rjzak on 2024-08-23 16:46_

Yes, sorry I missed that. It's also not clear that issue is with the released binaries from the GitHub releases page or something else, but I think aarch64 has the same issue with some distributions but I'm not sure.

This is an issue with the ppc64le binaries on the releases page.

---

_Label `bug` added by @charliermarsh on 2024-08-25 02:32_

---

_Comment by @charliermarsh on 2024-08-25 02:33_

Any idea how to approach fixing this? Do we need to set a different page size during build?

---

_Comment by @rjzak on 2024-08-25 02:55_

I'd say just have a note in the readme, release notes, or the binary file name to indicate the binary expects 4k pages on ppc64 (and probably arm) where 4k isn't the default. As for how to have your build environment run with a 64k page size when you likely aren't running on bare metal, I don't know. Can Docker allow running with different page sizes?

---

_Label `release` added by @zanieb on 2024-08-26 16:48_

---

_Referenced in [astral-sh/uv#4647](../../astral-sh/uv/issues/4647.md) on 2024-08-26 16:49_

---

_Comment by @zanieb on 2024-08-26 16:50_

We do this

https://github.com/astral-sh/uv/blob/e097f948c9eca7fb09948d73ac03248363c8ce7a/.github/workflows/build-binaries.yml#L574

per

- https://github.com/gnzlbg/jemallocator/issues/170
- https://github.com/astral-sh/ruff/issues/3791

---

_Comment by @rjzak on 2024-08-26 18:52_

That looks good for ARM devices.

---

_Comment by @tom-miller1 on 2024-09-10 14:49_

Same results on an IBM Power9 server, both the downloaded artifact from the release page and the pip-installed binary fail in jemalloc. Have not yet tried to build from the source, but this is a blocker for using `uv` as a CI build and test target for our internal Python apps on ppc64le.

```
$ .venv/bin/uv
<jemalloc>: Unsupported system page size
<jemalloc>: Unsupported system page size
memory allocation of 56 bytes failed
Abort(coredump)

$ tail -4 /proc/cpuinfo
platform        : pSeries
model           : IBM,9009-22A
machine         : CHRP IBM,9009-22A
MMU             : Hash

$ uname -srvmio        
Linux 4.18.0-477.43.1.el8_8.ppc64le #1 SMP Thu Jan 4 09:45:15 EST 2024 ppc64le ppc64le GNU/Linux

$ /bin/getconf PAGESIZE
65536
```

---

_Comment by @rjzak on 2024-09-10 19:21_

Maybe build from source and cache the binary for your CI? The build is pretty simple and straightforward. 

---

_Comment by @tom-miller1 on 2024-09-11 11:16_

Looks like this was fixed for ppc64le in Ruff using the same build flags that are being passed to maturin for aarch64. Could the same fix be applied to uv?

https://github.com/astral-sh/ruff/issues/10073

---

_Comment by @zanieb on 2024-09-11 11:42_

I'm happy to review a PR and see if it fixes it.

---

_Comment by @charliermarsh on 2024-09-11 15:43_

Yeah we should apply those changes. I actually thought we already had them in the build script, but it turns out they're only applied to some other architectures and not PowerPC.

---

_Label `help wanted` added by @charliermarsh on 2024-09-11 15:43_

---

_Referenced in [astral-sh/uv#7298](../../astral-sh/uv/pulls/7298.md) on 2024-09-11 16:27_

---

_Closed by @charliermarsh on 2024-09-11 18:13_

---

_Closed by @charliermarsh on 2024-09-11 18:13_

---

_Referenced in [astral-sh/uv#10942](../../astral-sh/uv/issues/10942.md) on 2025-01-24 18:18_

---

_Referenced in [archlinuxarm/PKGBUILDs#2089](../../archlinuxarm/PKGBUILDs/pulls/2089.md) on 2025-02-09 01:31_

---

_Referenced in [Wilfred/difftastic#872](../../Wilfred/difftastic/issues/872.md) on 2025-08-21 12:51_

---
