---
number: 12594
title: how to install a dynamically updated wheel package
type: issue
state: open
author: pplmx
labels:
  - question
assignees: []
created_at: 2025-04-01T05:22:46Z
updated_at: 2025-04-03T01:59:32Z
url: https://github.com/astral-sh/uv/issues/12594
synced_at: 2026-01-07T13:12:18-06:00
---

# how to install a dynamically updated wheel package

---

_Issue opened by @pplmx on 2025-04-01 05:22_

### Question

I have a wheel package that is updated daily in a directory named "latest". Each day, the package version is unique, and the "latest" directory always contains only one version of the package. For example, the package URLs look like this:

- https://xxx.url/latest/app-1.0.0+build.20250322.whl
- https://xxx.url/latest/app-1.0.0+build.20250325.whl

I would like to install this wheel using UV sources without having to update the version manually every day. How can this be achieved?

### Platform

Linux

### Version

uv 0.6.11

---

_Label `question` added by @pplmx on 2025-04-01 05:22_

---

_Comment by @charliermarsh on 2025-04-01 12:58_

You can probably do something like:

```toml
[[tool.uv.index]]
name = "my-index"
url = "https://xxx.url/latest"
format = "flat"
explicit = true

[tool.uv.sources]
app = { index = "my-index" }
```

---

_Comment by @charliermarsh on 2025-04-01 12:58_

You would just need an `index.html` under `/latest` that lists all the links.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-01 12:58_

---

_Comment by @pplmx on 2025-04-03 01:59_

Hi @charliermarsh,

Thank you for your response, and apologies for the delayed reply. From what you mentioned, am I correct in understanding that an `index.html` file is mandatory in this scenario?

Unfortunately, I don't have control over the `latest` directory, so I'm unable to add an `index.html` there. Could you suggest any alternative approaches?

Thanks in advance for your help.

---
