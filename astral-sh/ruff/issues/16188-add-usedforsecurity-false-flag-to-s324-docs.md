```yaml
number: 16188
title: "Add `usedforsecurity=False` flag to `S324` docs"
type: issue
state: closed
author: Skylion007
labels:
  - documentation
assignees: []
created_at: 2025-02-16T15:56:28Z
updated_at: 2025-02-16T18:06:56Z
url: https://github.com/astral-sh/ruff/issues/16188
synced_at: 2026-01-12T15:54:55Z
```

# Add `usedforsecurity=False` flag to `S324` docs

---

_@Skylion007_

### Description

It seems like adding `usedforsecurity=False` flag properly suppresses the rule `S324`. This makes enabling it useful for [FIPS](https://en.wikipedia.org/wiki/Federal_Information_Processing_Standards) compliance of popular python projects. I only discovered this due to trial and error though so it would be useful to add it the documentation for the rule

---

_Label `documentation` added by @ntBre on 2025-02-16 17:02_

---

_Closed by @ntBre on 2025-02-16 18:06_

---
