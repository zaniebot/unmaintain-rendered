```yaml
number: 4993
title: uv requires Content-Length Header on wheels, breaking compatibility with some package repositories
type: issue
state: closed
author: philieeas
labels:
  - compatibility
  - great writeup
  - network
assignees: []
created_at: 2024-07-11T14:59:46Z
updated_at: 2024-07-12T03:33:22Z
url: https://github.com/astral-sh/uv/issues/4993
synced_at: 2026-01-10T05:31:37Z
```

# uv requires Content-Length Header on wheels, breaking compatibility with some package repositories

---

_Issue opened by @philieeas on 2024-07-11 14:59_

When using uv as a drop-in replacement for pip, I encountered an issue where uv fails to download packages from a custom package repository due to the absence of the Content-Length Header in the response. The registry is getting proxied by nginx and it seems that strips that header(I unfortunately can't change that fact).

## Steps to reproduce:

- Install uv and create a virtual environment.
- Run the command `uv pip install --index-url <custom_repository_url> <package_name>`(You need a server that doesn't send the `Content-Length` header)
- Observe the error message indicating that the Content-Length Header is missing from the response.
* Tested UV platform: Windows, Tried on Linux too
* Used UV version: uv 0.2.24 (527b711bc 2024-07-10)

### Error message:

```
error: Failed to download <package_name>
  Caused by: content-length header is missing from response
```

### Response headers:

The response headers from the custom package repository do not include the Content-Length Header, as shown below:

```
HTTP/1.1 200 OK
Server: nginx
Date: Thu, 11 Jul 2024 14:38:49 GMT
Content-Type: binary/octet-stream
Transfer-Encoding: chunked
Connection: keep-alive
Accept-Ranges: bytes
Cache-Control: max-age=0, private, must-revalidate
Etag: "9b2d35955a25abe3acb5c94aa54ad3b1"
Last-Modified: Thu, 11 Jul 2024 14:34:01 GMT
Vary: Origin
Referrer-Policy: strict-origin-when-cross-origin
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

### Expected behavior:

uv should be able to download packages from custom package repositories even if the Content-Length Header is not present in the response. pip does not require this header, and uv should strive to maintain compatibility with pip.

### Suggested solution:

Consider adding support for downloading packages without the Content-Length Header, or provide an option to disable the requirement for this header. This would allow uv to work seamlessly with custom package repositories that do not provide this header.



<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:



-->


---

_Label `compatibility` added by @zanieb on 2024-07-11 15:52_

---

_Label `network` added by @zanieb on 2024-07-11 15:52_

---

_Comment by @zanieb on 2024-07-11 16:01_

Thanks for the report.

This error is coming from a dependency 

https://github.com/prefix-dev/async_http_range_reader/blob/f65c0c94ca4e3035b8561ee2ff8b1d5221b66abb/src/lib.rs#L319-L326

which we invoke at

https://github.com/astral-sh/uv/blob/d833910a5d210d72690ed25b03f4211607585f07/crates/uv-client/src/registry_client.rs#L578

We'll need to fallback to downloading the entire file — which we do support but we might need to make some changes to the stream library to handle streaming without a content-length?

Note this is going to drastically impact performance as we need to inspect each wheel for metadata and this will bypass our optimizations that avoid downloading the full file.

---

_Label `great writeup` added by @zanieb on 2024-07-11 16:01_

---

_Comment by @zanieb on 2024-07-11 16:15_

With a little more investigation, it looks like `async_http_range_reader` needs the content length to allocate a memory map. It seems like a stretch to make it content-length agnostic? cc @baszalmstra (just a heads up)

I think we'd need a separate code path that doesn't do async streaming which sounds like a pain.



---

_Comment by @hoechenberger on 2024-07-11 16:53_

Seems to me like a problem the custom repository should fix 😅 and it would be an easy fix too, at least theoretically 

still doesn't help users who want to use uv as a drop-in replacement for pip, though 

---

_Comment by @charliermarsh on 2024-07-12 00:46_

I think we already have a fallback path that avoids the async reader. This should just be another case of that... I'll look into it.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-12 00:53_

---

_Closed by @charliermarsh on 2024-07-12 01:04_

---

_Closed by @charliermarsh on 2024-07-12 01:04_

---

_Comment by @zanieb on 2024-07-12 03:33_

Ah thanks @charliermarsh I thought we had that but somehow couldn't find the path to it in the code.

---
