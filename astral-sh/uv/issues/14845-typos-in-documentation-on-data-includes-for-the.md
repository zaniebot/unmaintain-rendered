```yaml
number: 14845
title: Typos in documentation on data includes for the uv-build backend
type: issue
state: closed
author: ReubenVandezande
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2025-07-23T13:23:30Z
updated_at: 2025-07-24T09:55:16Z
url: https://github.com/astral-sh/uv/issues/14845
synced_at: 2026-01-10T03:32:45Z
```

# Typos in documentation on data includes for the uv-build backend

---

_Issue opened by @ReubenVandezande on 2025-07-23 13:23_

### Summary

The [documentation](https://docs.astral.sh/uv/reference/settings/#build-backend_data) on data inclusion for the build backend has two typos:

## Typo in purelib/platlib description ("uses" vs "use")
"purelib and platlib: Installed to the site-packages directory. It is not recommended to use~s~ these two options."

## Example uses python style dictionary when it should be in TOML format

```toml
[tool.uv.build-backend]
data = { "headers": "include/headers", "scripts": "bin" }
```

Should be 

```toml
[tool.uv.build-backend]
data = { "headers" = "include/headers", "scripts" = "bin" }
```


### Platform

N/A

### Version

N/A

### Python version

_No response_

---

_Label `bug` added by @ReubenVandezande on 2025-07-23 13:23_

---

_Label `bug` removed by @zanieb on 2025-07-23 13:26_

---

_Label `documentation` added by @zanieb on 2025-07-23 13:26_

---

_Label `good first issue` added by @zanieb on 2025-07-23 13:26_

---

_Closed by @konstin on 2025-07-24 09:55_

---
