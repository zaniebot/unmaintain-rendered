```yaml
number: 1309
title: Document font used in benchmark charts
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: main
head: roboto-font
created_at: 2024-02-15T07:53:40Z
updated_at: 2024-02-15T14:56:09Z
url: https://github.com/astral-sh/uv/pull/1309
synced_at: 2026-01-12T16:04:35Z
```

# Document font used in benchmark charts

---

_@MichaReiser_

The labels were missing when I generated the charts on my Arch machine. 

Inspecting the intermediate SVG showed that we use Google's Roboto font (and the rendering library doesn't support fallback fonts).

I installed the font and TADA, the labels appeared. I extended our documentation to mention the required fonts.  




---

_Label `documentation` added by @MichaReiser on 2024-02-15 08:18_

---

_@charliermarsh approved on 2024-02-15 14:56_

---

_Merged by @charliermarsh on 2024-02-15 14:56_

---

_Closed by @charliermarsh on 2024-02-15 14:56_

---

_Branch deleted on 2024-02-15 14:56_

---
