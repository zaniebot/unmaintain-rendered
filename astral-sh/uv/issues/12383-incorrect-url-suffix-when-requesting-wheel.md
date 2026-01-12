```yaml
number: 12383
title: Incorrect URL suffix when requesting wheel metadata in Simple HTML API
type: issue
state: closed
author: f3flight
labels:
  - bug
assignees: []
created_at: 2025-03-21T23:40:25Z
updated_at: 2025-03-22T15:36:51Z
url: https://github.com/astral-sh/uv/issues/12383
synced_at: 2026-01-12T16:01:02Z
```

# Incorrect URL suffix when requesting wheel metadata in Simple HTML API

---

_@f3flight_

### Summary

This may be a harmless bug, but wanted to report just because I noticed. When uv works with a HTML API (as opposed to json), it incorrectly appends wheel sha256 to the metadata URL. I can reproduce this using my org's internal pypi index which doesn't support JSON API.

The index response looks like this (truncated for brevity, part of url redacted):
```html
<!DOCTYPE html>
<body>
  <ul>
    <li><a href="https://<internal-storage>/some_package-0.4.9-py3-none-any.whl#sha256=794e49d06747a5c210d3e11ecc99930046348c278f7e53e5b87271962ba366da" data-core-metadata="sha256=9502b1c067d22065612e6b84a093a5a483a989e468031c5b8f38ec08a94aef92">some_package-0.4.9-py3-none-any.whl</a></li>
  </ul>
</body>
```
The URL uv uses looks like this:
```shell
UV_INDEX_URL=<redacted> uv pip compile <(echo some_package==0.4.9) -vvv | grep 'whl.metadata'
DEBUG No cache entry for: https://.../some_package-0.4.9-py3-none-any.whl.metadata#sha256=794e49d06747a5c210d3e11ecc99930046348c278f7e53e5b87271962ba366da
TRACE Sending fresh GET request for https://.../some_package-0.4.9-py3-none-any.whl.metadata#sha256=794e49d06747a5c210d3e11ecc99930046348c278f7e53e5b87271962ba366da
TRACE Handling request for https://.../some_package-0.4.9-py3-none-any.whl.metadata#sha256=794e49d06747a5c210d3e11ecc99930046348c278f7e53e5b87271962ba366da
TRACE Request for https://.../some_package-0.4.9-py3-none-any.whl.metadata#sha256=794e49d06747a5c210d3e11ecc99930046348c278f7e53e5b87271962ba366da is unauthenticated, checking cache
TRACE No credentials in cache for URL https://.../some_package-0.4.9-py3-none-any.whl.metadata#sha256=794e49d06747a5c210d3e11ecc99930046348c278f7e53e5b87271962ba366da
TRACE Attempting unauthenticated request for https://.../some_package-0.4.9-py3-none-any.whl.metadata#sha256=794e49d06747a5c210d3e11ecc99930046348c278f7e53e5b87271962ba366da
TRACE Cached request https://.../some_package-0.4.9-py3-none-any.whl.metadata#sha256=794e49d06747a5c210d3e11ecc99930046348c278f7e53e5b87271962ba366da is storable because its response has a heuristically cacheable status code 200
```
As you can see, the URL to fetch metadata uses `#sha256=...` suffix from wheel URL, instead of using the value from `data-core-metadata=`. In the end everything seems to work ok, but I don't know the internals well enough to say for sure.

This doesn't happen when using PyPI API, my guess is maybe because pypi api is a JSON API, so a slightly different code path is used, and in that path there's no hash suffix appended to whl.metadata request (based on what I see in the trace output)

### Platform

linux (azure linux 2)

### Version

0.6.9

### Python version

3.12.6

---

_Label `bug` added by @f3flight on 2025-03-21 23:40_

---

_Comment by @charliermarsh on 2025-03-22 00:06_

Thanks, it makes sense to strip these fragments.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-22 00:07_

---

_Closed by @charliermarsh on 2025-03-22 15:36_

---

_Closed by @charliermarsh on 2025-03-22 15:36_

---
