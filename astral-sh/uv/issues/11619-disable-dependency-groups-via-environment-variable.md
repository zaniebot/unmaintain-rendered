```yaml
number: 11619
title: Disable dependency groups via environment variable
type: issue
state: closed
author: abhiaagarwal
labels:
  - enhancement
  - configuration
assignees: []
created_at: 2025-02-19T13:22:24Z
updated_at: 2025-10-31T03:34:15Z
url: https://github.com/astral-sh/uv/issues/11619
synced_at: 2026-01-12T16:00:41Z
```

# Disable dependency groups via environment variable

---

_@abhiaagarwal_

### Summary

uv currently doesn't have a way to disable dependency groups via env variable. @zanieb proposed something like `UV_NO_DEV` in the linked bug below, It would be nice if this could be generalized to instead be something like `UV_EXCLUDE_GROUPS`, where the groups can be comma-separated. This has an advantage in a docker-based environment — by using an ARG + ENV combo, I can build multiple versions of a docker image, such as one with dev dependencies, or one with GPU dependencies.

I guess there's an argument that all of group-picking behavior including whitelisting should be controllable via an env variable, but that feels like it might cause weird hierarchy issues.

https://github.com/astral-sh/uv-docker-example/issues/36

### Example

```dockerfile
ARG UV_EXCLUDE_GROUPS=dev
ENV UV_EXCLUDE_GROUPS=${UV_EXCLUDE_GROUPS}

 # implies --no-group dev => --no-dev
uv sync ...

# ... I can build an image excluding gpu dependencies by passing `--build-arg UV_EXCLUDE_GROUPS=dev,gpu`
```


---

_Label `enhancement` added by @abhiaagarwal on 2025-02-19 13:22_

---

_Label `configuration` added by @zanieb on 2025-02-19 15:59_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-10-31 01:09_

---

_Closed by @zanieb on 2025-10-31 03:34_

---
