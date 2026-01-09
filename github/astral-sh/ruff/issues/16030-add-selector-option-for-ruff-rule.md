---
number: 16030
title: "Add `--selector` option for `ruff rule`"
type: issue
state: open
author: InSyncWithFoo
labels:
  - needs-decision
assignees: []
created_at: 2025-02-08T00:13:54Z
updated_at: 2025-03-20T03:43:04Z
url: https://github.com/astral-sh/ruff/issues/16030
synced_at: 2026-01-07T13:12:16-06:00
---

# Add `--selector` option for `ruff rule`

---

_Issue opened by @InSyncWithFoo on 2025-02-08 00:13_

There are multiple kinds of rule selectors:

```toml
[tool.ruff.lint]
select = [
	"ALL",     # All codes
	"Q000",    # Full code
	"TCH001",  # Redirected code
	"ANN0",    # Prefix
	"PLW",     # Linter and category
	"UP",      # Linter
	"E",       # Category
	"T"        # Cross-linter (legacy)
]
```

Out of all selectors above, only `Q000` is query-able using `ruff rule`. Thus, an informational popup like this can not be shown for the rest:

![](https://github.com/user-attachments/assets/d39eedde-ad43-4511-b234-d4dbb64d366e)

`ruff rule` should accept a `--selector` option that, when enabled, would cause Ruff to output a list of selected rules, similar to how `ruff config` handles a subkey.


---

_Label `needs-decision` added by @MichaReiser on 2025-02-08 07:56_

---

_Comment by @MichaReiser on 2025-02-08 07:57_

I don't think this is necessarily the scope of this command. Maybe a ruff rule list but not sure

---

_Comment by @InSyncWithFoo on 2025-02-09 07:36_

I ended up using `ruff check --show-settings --isolated --select SEL`. One drawback is that I have to check whether a selector is a full code or not on my own (using length and/or known code list), which leads to unwanted results for redirected codes.

---

_Referenced in [astral-sh/ruff#16056](../../astral-sh/ruff/issues/16056.md) on 2025-02-10 16:35_

---
