---
number: 11741
title: "Rename `src` directory"
type: issue
state: open
author: geoffrey-g-delhomme
labels:
  - question
  - external
assignees: []
created_at: 2025-02-24T10:46:51Z
updated_at: 2025-02-25T16:34:09Z
url: https://github.com/astral-sh/uv/issues/11741
synced_at: 2026-01-07T13:12:18-06:00
---

# Rename `src` directory

---

_Issue opened by @geoffrey-g-delhomme on 2025-02-24 10:46_

### Question

To match a predefined directory structure migrating to uv, I need to rename the `src` generated folder as `sources`. What are all the default configuration parameters I need to update / override in `pyproject.toml` ? Thank you !

### Platform

Linux 6.8.0-1021-azure x86_64 GNU/Linux

### Version

uv 0.6.1

---

_Label `question` added by @geoffrey-g-delhomme on 2025-02-24 10:46_

---

_Comment by @konstin on 2025-02-24 13:44_

What build backend are you using? The layout of your project is determined by the build backend, not by uv.

---

_Comment by @geoffrey-g-delhomme on 2025-02-25 16:23_

I am using hatchling. I can fix things with appropriate configuration in the following tags, but I wonder if there is a way to have things more seamless (avoiding the use of PYTHONPATH for example) as it is the case with `src` out of the box.

```toml
[tool.hatch.build.targets.sdist]
only-include=['sources', 'pyproject.toml', 'uv.lock', 'README.md']
[tool.hatch.build.targets.wheel]
packages = ["sources/mypackage"]
```

---

_Label `external` added by @konstin on 2025-02-25 16:34_

---
