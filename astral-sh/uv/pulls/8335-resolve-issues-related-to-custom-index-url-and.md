```yaml
number: 8335
title: Resolve issues related to custom index URL and environment variables
type: pull_request
state: closed
author: Aditya-PS-05
labels: []
assignees: []
base: main
head: bug/8324-auth-error-google-artifactory
created_at: 2024-10-18T15:20:30Z
updated_at: 2024-10-20T16:15:03Z
url: https://github.com/astral-sh/uv/pull/8335
synced_at: 2026-01-10T12:54:07Z
```

# Resolve issues related to custom index URL and environment variables

---

_Pull request opened by @Aditya-PS-05 on 2024-10-18 15:20_

closes #8324 

## Summary
I guess the `from_env` function from the credentials was not set in the middleware. So, used in middleware.

---

_@charliermarsh reviewed on 2024-10-18 15:21_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:308 on 2024-10-18 15:21_

I don't think this is quite right -- the index name is not necessarily the same as the URL host, it's a user-provided string.

---

_@Aditya-PS-05 reviewed on 2024-10-20 07:40_

---

_Review comment by @Aditya-PS-05 on `crates/uv-auth/src/middleware.rs`:308 on 2024-10-20 07:40_

@charliermarsh , 
Can you help me by telling me the position of getting index_name by the url?

---

_Comment by @charliermarsh on 2024-10-20 16:15_

I think this was fixed separately by https://github.com/astral-sh/uv/pull/8345 -- thanks for the PR, sorry for the overlap!

---

_Closed by @charliermarsh on 2024-10-20 16:15_

---
