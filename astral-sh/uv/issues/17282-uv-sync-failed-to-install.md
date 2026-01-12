```yaml
number: 17282
title: "uv sync failed to install \".\""
type: issue
state: closed
author: SafetyMary
labels:
  - question
assignees: []
created_at: 2026-01-02T03:09:38Z
updated_at: 2026-01-06T03:55:03Z
url: https://github.com/astral-sh/uv/issues/17282
synced_at: 2026-01-12T16:02:48Z
```

# uv sync failed to install "."

---

_@SafetyMary_

### Question

I have executed ```uv add . --dev --editable``` and then ```uv sync```. However the package does not appear under ```uv pip list```. 

As a workaround I have to execute ```uv pip install . -e``` everytime after ```uv sync```

Am I doing this wrong or if this is intended behaviour?

### Platform

Linux 6.18.2-1-default x86_64 GNU/Linux (OpenSUSE TW)

### Version

uv 0.9.15

---

_Label `question` added by @SafetyMary on 2026-01-02 03:09_

---

_Comment by @charliermarsh on 2026-01-06 00:10_

Does your package have a `[build-system]` table? See: https://docs.astral.sh/uv/concepts/projects/config/#build-systems

---

_Comment by @zanieb on 2026-01-06 00:18_

You shouldn't need to add your own project (e.g., `uv add .`) as a dependency when using `uv sync`. The current directory is generally included, though it requires a build system as referenced by Charlie.

---

_Comment by @SafetyMary on 2026-01-06 03:55_

Works fine after adding ```build-system```. Thanks a lot of the heads up.

---

_Closed by @SafetyMary on 2026-01-06 03:55_

---
