---
number: 14892
title: Allow dev-time only entry points
type: issue
state: open
author: tuukkamustonen
labels:
  - enhancement
assignees: []
created_at: 2025-07-25T12:18:25Z
updated_at: 2025-07-25T17:39:04Z
url: https://github.com/astral-sh/uv/issues/14892
synced_at: 2026-01-10T01:25:50Z
---

# Allow dev-time only entry points

---

_Issue opened by @tuukkamustonen on 2025-07-25 12:18_

### Summary

Not sure if this is against some specification, but sometimes you want to define a dev-time only entry point, which isn't included in the generated package:

In this case, if you customize [commitizen](https://commitizen-tools.github.io/commitizen/) usage, you need to [define an entry point for your override](https://commitizen-tools.github.io/commitizen/customization/#migrating-from-legacy-plugin-format).

The problem is, with `pyproject.toml` having:

```toml
[project.entry-points.'commitizen.plugin']
cz_something = ...
```

The packaged artifact (`uv build --wheel`) has this declared in metadata. But the definition is intended only for local use, I don't want to include it in the generated package.

Maybe there's a workaround?

### Example

_No response_

---

_Label `enhancement` added by @tuukkamustonen on 2025-07-25 12:18_

---

_Comment by @zanieb on 2025-07-25 17:39_

I think this overlaps with #5903 

---
