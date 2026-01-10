```yaml
number: 12592
title: "fix parsing of `python-platform` in settings tomls"
type: pull_request
state: merged
author: Gankra
labels:
  - bug
assignees: []
merged: true
base: main
head: gankra/settings-py
created_at: 2025-03-31T21:27:32Z
updated_at: 2025-04-01T13:08:37Z
url: https://github.com/astral-sh/uv/pull/12592
synced_at: 2026-01-10T11:10:40Z
```

# fix parsing of `python-platform` in settings tomls

---

_Pull request opened by @Gankra on 2025-03-31 21:27_

serde needs to be told where to put underscores. someone clearly noticed this when adding attributes for schemars, but they need to be present for serde too and then schemars gets them for free.

Strictly speaking this would be a breaking change for anyone who noticed the parsing was messed up and worked around it. So we add aliases for backcompat, at least for a few releases.

Fixes #12590

---

_Label `bug` added by @Gankra on 2025-03-31 21:28_

---

_Renamed from "fix parsing of python-platform in settings tomls" to "fix parsing of `python-platform` in settings tomls" by @Gankra on 2025-03-31 21:28_

---

_Label `breaking` added by @Gankra on 2025-03-31 21:31_

---

_@charliermarsh approved on 2025-04-01 03:08_

Nice, thanks for jumping on this. I think it's probably worth adding an alias and removing it in the next breaking release, just as a best-practice.

---

_Label `breaking` removed by @Gankra on 2025-04-01 12:57_

---

_Comment by @Gankra on 2025-04-01 13:08_

Aliases added.

---

_Merged by @Gankra on 2025-04-01 13:08_

---

_Closed by @Gankra on 2025-04-01 13:08_

---

_Branch deleted on 2025-04-01 13:08_

---
