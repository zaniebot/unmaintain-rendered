```yaml
number: 6225
title: "allow specifying a different `exclude` for each rule category"
type: issue
state: closed
author: DetachHead
labels: []
assignees: []
created_at: 2023-08-01T06:51:01Z
updated_at: 2023-08-02T04:24:29Z
url: https://github.com/astral-sh/ruff/issues/6225
synced_at: 2026-01-12T15:54:45Z
```

# allow specifying a different `exclude` for each rule category

---

_@DetachHead_

in my project, i don't want the pydocstyle `D` errors to appear in files matching a certain pattern (eg. so that internal modules do not require documentation).

using pydocstyle, that can be accomplished like this:
```toml
[tool.pydocstyle]
match = '(?!_?test_).*\.py'
```
however there seems to be no way to do this in ruff, without also disabling all other categories using the `exclude` option.

perhaps something like:
```toml
[tool.ruff]
exclude = ['*.pyi'] # global excludes

[tool.ruff.pydocstyle]
exclude = ['test_*.py', '_*.py'] # exclude option for specific tools (in addition to the global exclude patterns)
```

---

_Renamed from "allow specifying `exclude` for each rule category" to "allow specifying a different `exclude` for each rule category" by @DetachHead on 2023-08-01 06:52_

---

_Comment by @zanieb on 2023-08-01 14:19_

Hey @DetachHead — does `per-file-ignores` address your use-case? https://beta.ruff.rs/docs/settings/#per-file-ignores I believe it supports globs.

---

_Comment by @DetachHead on 2023-08-02 04:24_

yeah it does, thanks! (although my use case is blocked atm due to #6262)

i guess i missed it because i was looking at the [Configuration](https://beta.ruff.rs/docs/configuration/) page, and didn't realize there's a separate [Settings](https://beta.ruff.rs/docs/settings/) page with more information. perhaps these two pages could be combined? or "Settings" could be a subcategory of "Configuration"?

---

_Closed by @DetachHead on 2023-08-02 04:24_

---
