---
number: 7844
title: RUF200 does not recognize Hatch context formatting
type: issue
state: open
author: scott-8
labels:
  - bug
assignees: []
created_at: 2023-10-07T00:50:51Z
updated_at: 2024-09-12T16:56:42Z
url: https://github.com/astral-sh/ruff/issues/7844
synced_at: 2026-01-07T13:12:15-06:00
---

# RUF200 does not recognize Hatch context formatting

---

_Issue opened by @scott-8 on 2023-10-07 00:50_

When Ruff tries to validate URLs using Hatch's context formatting, it fails where it shouldn't.

A file containing this snippet:
```
[project.optional-dependencies]
dev = ["local-package @ {root:uri}/local-package"]
```

Will give this error:
```
pyproject.toml:50:9: RUF200 Failed to parse pyproject.toml: relative URL without a base
local-package @ {root:uri}/local-package
             ^^^^^^^^^^^^^^^^^^^^^
Found 1 error.
```
I'm using the latest Ruff (0.0.292). Here is some documentation from Hatch:
Context formatting: https://hatch.pypa.io/latest/config/context/
How they're used: https://hatch.pypa.io/latest/config/dependency/#local

---

_Comment by @charliermarsh on 2023-10-07 01:26_

Thanks, I'm surprised this hasn't come up before!

---

_Label `bug` added by @charliermarsh on 2023-10-08 14:41_

---

_Comment by @gabinorsworthy on 2024-08-08 18:25_

Is there any update on this? Or any workaround? I'm seeing the same error.

---

_Comment by @charliermarsh on 2024-08-08 18:27_

Unfortunately not at the moment. I think your best bet is to disable the rule. What Hatch is doing here isn't standards-compliant and is Hatch-specific, so to support it properly we'd have to reimplement a Hatch-specific feature.

---

_Comment by @Phlogistique on 2024-09-12 16:56_

There is a workaround! You can do this:

```toml
[project.optional-dependencies]
dev = ["local-package @ file://{root}/local-package"]
```

---
