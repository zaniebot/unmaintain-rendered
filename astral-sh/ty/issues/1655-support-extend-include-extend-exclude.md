```yaml
number: 1655
title: "Support `extend-include`, `extend-exclude`"
type: issue
state: open
author: alexei
labels:
  - question
  - configuration
assignees: []
created_at: 2025-11-27T12:49:41Z
updated_at: 2025-11-29T14:57:28Z
url: https://github.com/astral-sh/ty/issues/1655
synced_at: 2026-01-12T15:54:25Z
```

# Support `extend-include`, `extend-exclude`

---

_@alexei_

It would be useful if `[tool.ty.src]` allowed `extend-include` and `extend-exclude` directives. `include` and `exclude` have some defaults which work for most packages (or at least those using the "src" layout). But sometimes there's _that_ package that has a `scripts/` directory or something. Now the config looks like:

```toml
[tool.ty.src]
include = [
    "scripts/",
    "src/",
    "tests/",
]
```

With `extend-include` it would be simplified to:

```toml
[tool.ty.src]
extend-include = [
    "scripts/",
]
```

This would also be very familiar to Ruff users, see https://docs.astral.sh/ruff/settings/#extend-include

---

As a side note, it is strange to me that both `include` and `exclude` default to `null` (or so are documented) when they're not really null.


---

_Comment by @MichaReiser on 2025-11-27 13:09_

Both `include` and `exclude` have the `extend` semantics from Ruff in that additional entries will be appended. However, this isn't very useful at the moment because ty doesn't support hierarchical configuration files. 

To remove an `exclude` in ty, you specify a negative exclude pattern. That will take precedence over the default exclude. See https://docs.astral.sh/ty/reference/configuration/#exclude_1 for more details




---

_Label `question` added by @MichaReiser on 2025-11-27 13:09_

---

_Label `configuration` added by @AlexWaygood on 2025-11-27 13:15_

---

_Comment by @alexei on 2025-11-29 09:42_

To me, changing semantics from one project to another is very confusing. I think that should be avoided or at least documented.

---

_Comment by @MichaReiser on 2025-11-29 14:57_

The new semantics are documented, but happy to make any changes if you can suggest a specific improvement.

We didn't change this just for fun ;).  Ruff's behavior has consistently been confusing to users, and the new behavior is much closer to uv's include/exclude semantics. 

---
