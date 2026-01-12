```yaml
number: 17015
title: "[red-knot] Fix syntax highlighting for pyi files"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/playground-pyi
created_at: 2025-03-27T17:10:02Z
updated_at: 2025-03-27T19:27:23Z
url: https://github.com/astral-sh/ruff/pull/17015
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Fix syntax highlighting for pyi files

---

_@MichaReiser_

Monaco supports inferring the language based on the file's extension but it doesn't seem to support `pyi`. I tried to patch up the python language definition by adding `.pyi` to the language's `extension` array but that didn't work. That's why I decided to patch up the language in React.

---

_Label `playground` added by @MichaReiser on 2025-03-27 17:10_

---

_Label `red-knot` added by @MichaReiser on 2025-03-27 17:10_

---

_Merged by @MichaReiser on 2025-03-27 19:27_

---

_Closed by @MichaReiser on 2025-03-27 19:27_

---

_Branch deleted on 2025-03-27 19:27_

---
