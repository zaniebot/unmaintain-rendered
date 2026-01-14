```yaml
number: 2487
title: "Unable to pull `latest` (or any other) container image"
type: issue
state: open
author: frodera
labels: []
assignees: []
created_at: 2026-01-14T03:43:21Z
updated_at: 2026-01-14T03:43:21Z
url: https://github.com/astral-sh/ty/issues/2487
synced_at: 2026-01-14T04:30:21Z
```

# Unable to pull `latest` (or any other) container image

---

_@frodera_

### Summary

Following the instructions in [Installing in Docker](https://docs.astral.sh/ty/installation/#installing-in-docker) fails with: 

```
% docker pull ghcr.io/astral-sh/ty:latest
Error response from daemon: Head "https://ghcr.io/v2/astral-sh/ty/manifests/latest": unauthorized
```

> Install ty in Docker by copying the binary from the official image:
> Dockerfile
> 
> COPY --from=ghcr.io/astral-sh/ty:latest /ty /bin/
> 
> The following tags are available:
> 
>     ghcr.io/astral-sh/ty:latest
> [...]

### Version

_No response_

---
