```yaml
number: 1875
title: Non-rendered callout in documentation
type: issue
state: closed
author: eggplants
labels:
  - bug
  - documentation
assignees: []
created_at: 2025-12-13T09:13:00Z
updated_at: 2025-12-14T09:21:56Z
url: https://github.com/astral-sh/ty/issues/1875
synced_at: 2026-01-12T15:54:26Z
```

# Non-rendered callout in documentation

---

_@eggplants_

### Summary

GFM callout syntax is not rendered in mkdocs. it requires [markdown-callouts](https://github.com/oprypin/markdown-callouts#support-github-alerts-style-of-admonitions).

https://docs.astral.sh/ty/reference/configuration/#root_1

<img width="731" height="272" alt="Image" src="https://github.com/user-attachments/assets/9509e71d-991a-4937-a385-31f80bf2525b" />

### Version

ty 0.0.1-alpha.34

---

_Label `bug` added by @AlexWaygood on 2025-12-13 12:16_

---

_Label `documentation` added by @AlexWaygood on 2025-12-13 12:16_

---

_Added to milestone `Beta` by @AlexWaygood on 2025-12-13 12:16_

---

_Comment by @leandrobbraga on 2025-12-13 16:53_

I implemented two solutions: [one](https://github.com/astral-sh/ty/pull/1879) that adds a new dependency and renders the callout with the current syntax and [another](https://github.com/astral-sh/ruff/pull/21961) that changes the callout syntax to the currently supported version.

I prefer the second option because it doesn’t add a dependency.

---

_Closed by @MichaReiser on 2025-12-14 09:21_

---
