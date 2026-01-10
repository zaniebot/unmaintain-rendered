---
number: 2253
title: "Nexus doesn't return a `Last-Modified` header"
type: issue
state: open
author: charliermarsh
labels:
  - registry
  - needs-mre
  - cache
assignees: []
created_at: 2024-03-06T23:21:58Z
updated_at: 2024-05-26T00:42:32Z
url: https://github.com/astral-sh/uv/issues/2253
synced_at: 2026-01-10T01:23:15Z
---

# Nexus doesn't return a `Last-Modified` header

---

_Issue opened by @charliermarsh on 2024-03-06 23:21_

Okay, so it looks like we're failing to use the cache because:

- `new_policy.response.headers.last_modified_unix_timestamp` is `None`...
- But `self.response.headers.last_modified_unix_timestamp` is `Some(1709511660)`.

So the server isn't returning a `last_modified_unix_timestamp`. My read is that technically we're not supposed to use the cached value here, based on https://www.rfc-editor.org/rfc/rfc9111.html#section-4.3.4?

\cc @BurntSushi

_Originally posted by @charliermarsh in https://github.com/astral-sh/uv/issues/1754#issuecomment-1981790355_
            

---

_Label `registry` added by @charliermarsh on 2024-03-06 23:22_

---

_Comment by @BurntSushi on 2024-03-07 16:24_

I looked into this, and as far as I can tell, we are following the relevant HTTP caching specifications correctly. Notably, we should be [setting the `If-None-Match` and `If-Modified-Since`](https://github.com/astral-sh/uv/blob/4796927c4c61b2508c850c03d24f36647513e628/crates/uv-client/src/httpcache/mod.rs#L507-L546) headers when building our revalidation request. But it turns out that even though our cached response has a `Last-Modified` header, the revalidation response (status=304) we get back does not, seemingly.

I think there are probably two ways forward here:

1. Dig into the problem more and try to reproduce the issue with Nexus. IIRC, @charliermarsh tried this but couldn't manage to reproduce the problem. In particular, one possibility is that we aren't doing something correctly in our revalidation request that is causing the registry to omit the `Last-Modified` header.
2. We could diverge from the HTTP caching specs and treat a 304 as unconditionally implying that the stored response is still fresh when it doesn't have any validators on it, regardless of whether the cached response has any validators. (That is, if our cached response _and_ our revalidation response were both missing `E-Tag` and `Last-Modified` headers, then we'd treat the 304 as indicating that our cached response is fresh. But the fact that the cached response has it and the revalidation response doesn't is what fouls things up here. So we could relax this a little bit.) But I'm hesitant to diverge from the spec here.

It's unclear how broadly this is impacting things, so I'm going to stop here for now until I have more data. It's also possible that this was an issue on Nexus' end that is fixed in a newer version.

---

_Label `needs-mre` added by @BurntSushi on 2024-03-07 16:25_

---

_Label `cache` added by @BurntSushi on 2024-03-07 16:25_

---

_Comment by @kendallbailey on 2024-03-13 16:22_

Here's another data point related to Nexus and caching. I published a new version of a package in Nexus, and `pip` could install it but `uv` insisted it didn't exist until I deleted the related `.rkyv` file.

I'm using `uv` 0.1.17

```
$ uv pip install --no-deps ray-torch-meta==1.2
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of ray-torch-meta==1.2 and you require ray-torch-meta==1.2, we can conclude that the
      requirements are unsatisfiable.

$ rm ~/.cache/uv/simple-v3/ede79a3b4249d4c4/ray-torch-meta.rkyv

$ uv pip install --no-deps ray-torch-meta==1.2
Resolved 1 package in 106ms
Downloaded 1 package in 37ms
Installed 1 package in 2ms
 - ray-torch-meta==1.1
 + ray-torch-meta==1.2
```

---

_Comment by @charliermarsh on 2024-03-13 16:24_

You can run with `--refresh` to ensure we go back to fetch fresh data.

---

_Comment by @bewinsnw on 2024-05-26 00:41_

FWIW, I just investigated a similar problem with simple-repository-server (https://github.com/simple-repository/simple-repository-server).  TLDR; this is fixed at the bottom of this comment, which is just here to show how.

I'm trying to use that because it claims to act as a proxy that will add things like PEP-658 support to PEP-503 repos. It does do that and I'm seeing some nice speedups now that uv looks at data-core-metadata in index files; but I do still see some odd reactions in uv to their 304s.

```
DEBUG Found modified response for: http://localhost:9191/resources/nwpy-awskms/nwpy_awskms-1.2.6-py3-none-any.whl.metadata
WARN Server returned unusable 304 for: http://localhost:9191/resources/nwpy-awskms/nwpy_awskms-1.2.6-py3-none-any.whl.metadata
DEBUG Found modified response for: http://localhost:9191/resources/nwpy-app-config/nwpy_app_config-1.10.2-py3-none-any.whl.metadata
WARN Server returned unusable 304 for: http://localhost:9191/resources/nwpy-app-config/nwpy_app_config-1.10.2-py3-none-any.whl.metadata
DEBUG Found modified response for: http://localhost:9191/resources/nwpy-logging/nwpy_logging-2.19.6-py3-none-any.whl.metadata
WARN Server returned unusable 304 for: http://localhost:9191/resources/nwpy-logging/nwpy_logging-2.19.6-py3-none-any.whl.metadata
```

in the code, when it 304s, it does it mostly by raising an HttpException(304) eg https://github.com/simple-repository/simple-repository-server/blob/main/simple_repository_server/routers/simple.py#L158
this doesn't match what 304s are supposed to do,

```
The server generating a 304 response MUST generate any of the following header fields that would have been sent in a [200 (OK)](https://www.rfc-editor.org/rfc/rfc9110#status.200) response to the same request:

[Content-Location](https://www.rfc-editor.org/rfc/rfc9110#field.content-location), [Date](https://www.rfc-editor.org/rfc/rfc9110#field.date), [ETag](https://www.rfc-editor.org/rfc/rfc9110#field.etag), and [Vary](https://www.rfc-editor.org/rfc/rfc9110#field.vary)
Cache-Control and Expires (see [[CACHING](https://www.rfc-editor.org/rfc/rfc9110#CACHING)])
```
(from https://www.rfc-editor.org/rfc/rfc9110#name-304-not-modified) but it's not clear to me what uv is complaining about, specifically. In the responses from the server that produced the errors above,  the responses are pretty short:

```
$ curl -v -H 'if-none-match: "c8b73eca5053"' http://localhost:9191/resources/nwpy-logging/nwpy_logging-2.19.6-py3-none-any.whl.metadata  
*   Trying [::1]:9191...
* Connected to localhost (::1) port 9191
> GET /resources/nwpy-logging/nwpy_logging-2.19.6-py3-none-any.whl.metadata HTTP/1.1
> Host: localhost:9191
> User-Agent: curl/8.4.0
> Accept: */*
> if-none-match: "c8b73eca5053"
> 
< HTTP/1.1 304 Not Modified
< date: Sat, 25 May 2024 13:00:42 GMT
< server: uvicorn
```
in this case, the backend server behind the proxy does not send etag, vary, cache-control or expires; the proxy generated the etag. So the proxy response is incorrect in not replying with the original etag in the 304. Last modified mentioned in this bug is always optional, and the rfc says no other headers than the 5 above are supposed to matter? In all other respects, this looks ok. 

And long story short, patching the code to add back the etag to the 304 responses worked - the uv logs now look like:
```
DEBUG Selecting: nwpy-app-config==1.10.2 (nwpy_app_config-1.10.2-py3-none-any.whl)
DEBUG Found not-modified response for: http://localhost:9191/resources/nwpy-awskms/nwpy_awskms-1.2.6-py3-none-any.whl.metadata
DEBUG Found not-modified response for: http://localhost:9191/resources/nwpy-app-config/nwpy_app_config-1.10.2-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for nwpy-app-config==1.10.2: jinja2>=2.10.1, <4
```

no warning, no spurious extra requests. Fixing this saved an extra .2s on the uv pip install (pip install with warm cache was 21.8s, uv with cold cache was 6.2s, uv with warm cache was 2.2s, now uv with warm cache is 2.0s)

Nothing for uv to act on here - leaving the note for any other repository implementers wondering about the "unusuable 304s"

---
