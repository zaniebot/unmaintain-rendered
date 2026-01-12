```yaml
number: 9675
title: "[Question]Is there any conditional package dependency setting when dev vs build"
type: issue
state: closed
author: hawksung1
labels: []
assignees: []
created_at: 2024-12-06T11:50:25Z
updated_at: 2024-12-06T11:58:29Z
url: https://github.com/astral-sh/uv/issues/9675
synced_at: 2026-01-12T15:59:56Z
```

# [Question]Is there any conditional package dependency setting when dev vs build

---

_@hawksung1_

I have two packages in my project. However, they need to be deployed separately during actual deployment.
I am using them as shown below for debugging during development.
Is there a way to integrate them together at once?

when run build (for debugging)
```
[tool.uv.sources]
asdf = { workspace = true }
```

when build
```
[tool.uv.sources]
asdf = {path = "/tmp/asdf-0.1-py3-none-any.whl" }
```






---

_Comment by @charliermarsh on 2024-12-06 11:58_

I think this is roughly the same as https://github.com/astral-sh/uv/issues/7945. Unfortunately there isn't a great way to do it today. I'm going to expand that issue.

---

_Closed by @charliermarsh on 2024-12-06 11:58_

---
