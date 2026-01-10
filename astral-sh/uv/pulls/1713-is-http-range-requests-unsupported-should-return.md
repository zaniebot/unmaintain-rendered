```yaml
number: 1713
title: is_http_range_requests_unsupported should return true on Method Not Allowed
type: pull_request
state: merged
author: olivierlefloch
labels: []
assignees: []
merged: true
base: main
head: olivier/survive_http_405_on_head_reqs
created_at: 2024-02-19T19:09:20Z
updated_at: 2024-02-19T21:38:43Z
url: https://github.com/astral-sh/uv/pull/1713
synced_at: 2026-01-10T15:33:24Z
```

# is_http_range_requests_unsupported should return true on Method Not Allowed

---

_Pull request opened by @olivierlefloch on 2024-02-19 19:09_

## Summary

Azure Artifacts does not allow HEAD requests when attempting to download packages. This expands error handling in `is_http_range_requests_unsupported` to identify HTTP 405 (Method Not Allowed) error codes, and return `true` (i.e. Range requests will not be supported). This partially addresses #1458 – after this change, Azure Artifacts downloads still fail, but due to 401 Not Authorized instead of 405 Method Not Allowed.

## Test Plan

I ran something akin to

```
RUST_LOG=trace cargo run -- pip install --index-url=https://REDACTED:REDACTED@pkgs.dev.azure.com/REDACTED/_packaging/REDACTED/pypi/simple/ --upgrade --verbose private-package
```

without this code, and got a 405 failure:

```
error: Failed to download: private-package==1.2.3
  Caused by: HTTP status client error (405 Method Not Allowed) for url (https://pkgs.dev.azure.com/REDACTED/_packaging/REDACTED/pypi/download/private-package/1.2.3/private_package-1.2.3-py3-none-any.whl#sha256=REDACTED)
  ```

with this code, I get a 401 failure:

```
error: Failed to download: private-package==1.2.3
  Caused by: HTTP status client error (401 Unauthorized) for url (https://pkgs.dev.azure.com/REDACTED/_packaging/REDACTED/pypi/download/private-package/1.2.3/private_package-1.2.3-py3-none-any.whl#sha256=REDACTED)
```

## Caveats

I'm not seeing a non HEAD request being reported as being fired, so I'm not sure I'm doing this correctly!

---

_@charliermarsh approved on 2024-02-19 19:10_

Thanks, this does look right to me.

---

_Comment by @charliermarsh on 2024-02-19 19:13_

I suspect the 401 is related to us somehow not preserving the credentials in the URL.

---

_Marked ready for review by @charliermarsh on 2024-02-19 20:00_

---

_Comment by @charliermarsh on 2024-02-19 20:12_

@olivierlefloch - Okay from your perspective if I merge?

---

_Comment by @olivierlefloch on 2024-02-19 20:40_

Absolutely! I was unsure if this needed tests, etc., but I think it solves part of the issue!

---

_Merged by @charliermarsh on 2024-02-19 20:40_

---

_Closed by @charliermarsh on 2024-02-19 20:40_

---

_Comment by @charliermarsh on 2024-02-19 20:40_

We're still figuring out our testing infra for private registries, so it's "testing live" for now :)

---

_Branch deleted on 2024-02-19 21:38_

---
