```yaml
number: 19573
title: "ruff --watch improvements: ignore dirs, debouncee"
type: issue
state: open
author: sathishvj
labels:
  - cli
  - wish
assignees: []
created_at: 2025-07-27T10:47:38Z
updated_at: 2026-01-16T08:57:13Z
url: https://github.com/astral-sh/ruff/issues/19573
synced_at: 2026-01-16T10:07:21Z
```

# ruff --watch improvements: ignore dirs, debouncee

---

_@sathishvj_

### Summary

Summary
Improve the developer experience of ruff --watch by adding the ability to:

1. Ignore specific directories or files
2. Debounce file change events to reduce redundant action on rapid save operations or bulk changes

1. Ignore Directories or Files
Problem:
Currently, it seems that `ruff --watch` triggers on all file changes. Again, it seems to be me that `--exclude` does not apply linting rules, but `--watch` still triggers.

- Unnecessary linter runs
- Jankiness/Flashing
- Excessive CPU usage

Proposed Solution:
Introduce a CLI flag (and/or config option in pyproject.toml) to ignore specific directories or glob patterns. Example:

```bash
ruff --watch --ignore-dirs node_modules,.venv,build,dist```

pyproject.toml:

``toml
[tool.ruff.watch]
ignore = ["node_modules", ".venv", "build", "dist"]
```

2. Debounce File Events
similar to the above, to reduce overwork on fast file changes.

### Version

_No response_

---

_Label `cli` added by @MichaReiser on 2025-07-28 09:36_

---

_Label `wish` added by @MichaReiser on 2025-07-28 09:36_

---

_Comment by @MichaReiser on 2025-07-28 09:38_

Thanks. Debouncing and only running ruff when a project file (not matching exclude and matches include) does make sense to me. 

We could try to reuse ty's file watcher. It implements debouncing

---

_Comment by @leonace924 on 2026-01-15 14:12_

@MichaReiser is this fixed?

---

_Comment by @MichaReiser on 2026-01-15 14:14_

I don't think so

---

_Comment by @leonace924 on 2026-01-15 14:16_

@MichaReiser  may I work on this as well? would you assign to me?
I am gonna put the PR today as I put the one already

---

_Comment by @MichaReiser on 2026-01-16 08:56_

Sure. You may be interested in https://github.com/astral-sh/ruff/blob/5b240fbe72151c93129469b7534cc838c721c696/crates/ty_project/src/watch/watcher.rs#L209 which has been working pretty well in ty. I don't think we need the same degree of complexity but it may help to understand which events we can skip

---
