---
number: 16959
title: uv sync cannot cache the nvidia wheel packages
type: issue
state: closed
author: pplmx
labels:
  - bug
assignees: []
created_at: 2025-12-03T09:28:21Z
updated_at: 2025-12-20T04:59:16Z
url: https://github.com/astral-sh/uv/issues/16959
synced_at: 2026-01-10T01:26:11Z
---

# uv sync cannot cache the nvidia wheel packages

---

_Issue opened by @pplmx on 2025-12-03 09:28_

### Summary

```bash
uv init /var/tmp/kk -p 3.12 && cd /var/tmp/kk
uv add requests
uv add tensorrt-cu12 --index nv=https://pypi.nvidia.com/ --optional cu12
uv sync # it will remove the above tensorrt* packages, which is expected
uv sync --extra cu12 # it will download again, which is unexpected
```

### Platform

Ubuntu 22.04.5 LTS

### Version

uv 0.9.10

### Python version

Python 3.12.12

---

_Label `bug` added by @pplmx on 2025-12-03 09:28_

---

_Renamed from "uv sync cannot cache the nvidia packages that installed by "uv add tensorrt-cu12 --index nv=https://pypi.nvidia.com/ --optional cu12"" to "uv sync cannot cache the nvidia wheel packages" by @pplmx on 2025-12-10 01:56_

---

_Comment by @konstin on 2025-12-17 17:25_

That's strange, that shouldn't happen.

---

_Comment by @charliermarsh on 2025-12-17 17:27_

Are there any cache headers on that index?

---

_Comment by @konstin on 2025-12-17 17:36_

I see this in the CLI logs (I only checked if I can reproduce, no investigation yet):

```
DEBUG Identified uncached distribution: tensorrt-cu12-bindings==10.14.1.48.post1
DEBUG Identified uncached distribution: tensorrt-cu12-libs==10.14.1.48.post1
DEBUG Identified uncached distribution: cuda-toolkit==12.9.1
DEBUG Identified uncached distribution: nvidia-cuda-runtime-cu12==12.9.79
```

The headers (random file for sample) look good, they are doing both last-modified and etag:

```
$ curl -I https://pypi.nvidia.com/tensorrt-cu12/tensorrt_cu12-10.7.0.tar.gz#sha256=a9d3119613aeb1c27e7157bf05b110f36b9ab4e85a84c843d4f6248e2648cff5
HTTP/2 200 
content-type: application/x-gzip
content-length: 18327
last-modified: Tue, 03 Dec 2024 07:59:22 GMT
x-amz-server-side-encryption: AES256
x-amz-version-id: null
accept-ranges: bytes
server: AmazonS3
etag: "e742c9ca30dd7563337185612fce1bd2"
x-amz-cf-pop: TXL52-P2
x-amz-cf-id: oDDAOX0AlaAZVKA3a1t_kZLJ-RsHGcD8sxc9zRlkC5eddbwQbhtiMg==
expires: Wed, 17 Dec 2025 17:34:30 GMT
cache-control: max-age=0, no-cache, no-store
pragma: no-cache
date: Wed, 17 Dec 2025 17:34:30 GMT
x-cdn-version: v1
x-cdn: akam
akamai-grn: 0.a741c717.1765992870.1b6d3b
```

---

_Comment by @charliermarsh on 2025-12-17 17:44_

But they have no-cache and no-store on it. This seems right?

---

_Comment by @charliermarsh on 2025-12-17 18:14_

I would expect this to resolve it:

```toml
[[tool.uv.index]]
name = "nvidia"
url = "https://pypi.nvidia.com"
cache-control = { api = "max-age=600", files = "max-age=365000000, immutable" }
```

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-17 18:15_

---

_Referenced in [astral-sh/uv#17164](../../astral-sh/uv/pulls/17164.md) on 2025-12-17 18:23_

---

_Comment by @pplmx on 2025-12-18 12:00_

> I would expect this to resolve it:
> 
> [[tool.uv.index]]
> name = "nvidia"
> url = "https://pypi.nvidia.com"
> cache-control = { api = "max-age=600", files = "max-age=365000000, immutable" }

@charliermarsh 
Works fine, thanks a lot!

Just to confirm: once #17164 is merged, the default behavior should handle this correctly, so users won’t need to explicitly set `cache-control` for the NVIDIA index anymore — is that right?


---

_Comment by @konstin on 2025-12-18 12:16_

I've also raised this with the maintainers of the nvidia index.

---

_Closed by @charliermarsh on 2025-12-18 13:33_

---

_Comment by @pplmx on 2025-12-19 01:56_

> I've also raised this with the maintainers of the nvidia index.

Could you share the upstream issue link so we can track it?

---

_Comment by @konstin on 2025-12-19 12:23_

I don't think there is a public link I could share.

---

_Comment by @pplmx on 2025-12-20 04:59_

> I don't think there is a public link I could share.

Got it, thanks for the clarification! Please let us know if anything changes or if there’s an update from the maintainers.

---
