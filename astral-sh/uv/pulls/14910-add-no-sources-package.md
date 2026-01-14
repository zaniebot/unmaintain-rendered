```yaml
number: 14910
title: "Add `--no-sources-package`"
type: pull_request
state: open
author: zanieb
labels:
  - cli
assignees: []
base: main
head: zb/no-sources-package
created_at: 2025-07-25T21:11:34Z
updated_at: 2026-01-14T17:50:52Z
url: https://github.com/astral-sh/uv/pull/14910
synced_at: 2026-01-14T18:48:19Z
```

# Add `--no-sources-package`

---

_@zanieb_

I needed this for a test, e.g., to disable a source for an extra build dependency without disabling the source for a workspace member, and had also seen some requests for it. I think it makes sense to allow this.

The refactor is fairly mechanical, we go from `SourceStrategy::Enabled|Disabled` to `NoSources::All|None|Package(names)` as we do for other options like `NoBinary`.

Related https://github.com/astral-sh/uv/issues/17441

---

_Label `cli` added by @zanieb on 2025-07-25 21:11_

---

_Marked ready for review by @zanieb on 2026-01-14 17:50_

---
