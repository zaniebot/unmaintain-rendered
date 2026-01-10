---
number: 8580
title: Suggestion to improve downloads from registries that expose the METADATA file (PEP 658)
type: issue
state: open
author: mmarston
labels:
  - performance
assignees: []
created_at: 2024-10-25T22:44:16Z
updated_at: 2024-10-28T08:44:48Z
url: https://github.com/astral-sh/uv/issues/8580
synced_at: 2026-01-10T01:24:30Z
---

# Suggestion to improve downloads from registries that expose the METADATA file (PEP 658)

---

_Issue opened by @mmarston on 2024-10-25 22:44_

I was looking into range support on a registry server and looking at the sequence of requests that `uv` makes. Currently, when a registry server doesn't support PEP 658, but does support range requests, then `uv` will make 3 or 4 different requests for the wheel:

1. a HEAD request to initialize the `AsyncRangeReader` (used to get the size and check for `Accept-Range`)
2. a range GET request to get the tail of the wheel including the ZIP directory.
3. the AsyncRangeReader is used to read the range of the ZIP file containing metadata file. This may or may not require another HTTP request if the metadata file is located within the range returned in step 2. 
4. a GET request to download the full wheel

Steps 1 - 3 are performed during resolution and step 4 is performed during preparation.

When the registry server doesn't support range requests then 2 requests are made:

1. a HEAD request to initialize the `AsyncRangeReader`, which in this case is used to determine that the range approach cannot be used
2. a GET request to download the full wheel

And from what I gather from closed issues (such as #5073), there were versions of `uv` that made separate GET requests during resolution and preparation (so that the request during resolution could be aborted after encountering the metadata file).

My suggestion is that instead of starting with a HEAD request, start with a single tail range request. If the server returns a 206 response indicating that the server supports range requests, then use that response to initialize the `AsyncRangeReader`. If the server doesn't support range requests then it will return a 200 response including the full file. This approach makes the HEAD request unnecessary, saving one round trip request, whether or not the server supports range requests.

Something else you may consider is increasing the size of the initial tail range request (currently 16 KiB from what I see [here](https://github.com/astral-sh/uv/blob/main/crates/uv-client/src/remote_metadata.rs#L57C41-L57C46)). If the Content-Range response header on the initial tail response shows that the full file is returned then you can follow the same code path as the 200 response above: you can save and cache the file and won't need to make a 2nd request. You'd have to do some analysis to determine whether this is worthwhile, and what size to use, but I wouldn't be surprised if a 128 KiB tail covered a sizable percentage of small packages and only adds minimal extra to the round trip transfer time compared with downloading 16 KiB.



---

_Label `performance` added by @zanieb on 2024-10-25 23:48_

---

_Comment by @zanieb on 2024-10-27 13:59_

cc @konstin / @charliermarsh

---

_Comment by @konstin on 2024-10-28 08:44_

CC @baszalmstra who's the expert for `async_http_range_reader`

> My suggestion is that instead of starting with a HEAD request, start with a single tail range request.

iirc we had some server that supported range requests, but not negative range requests, so we'd need a HEAD request for the size of the file first.

> a GET request to download the full wheel

This is a bad case we really want to avoid, it makes the resolution very slow.

---
