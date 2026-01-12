```yaml
number: 19373
title: "chore: Document Material for MkDocs Insiders limitations in CONTRIBUTING.md in more detail"
type: pull_request
state: merged
author: corneliusroemer
labels:
  - documentation
  - internal
assignees: []
merged: true
base: main
head: material-mkdcos-insiders
created_at: 2025-07-15T19:12:57Z
updated_at: 2025-07-16T08:42:25Z
url: https://github.com/astral-sh/ruff/pull/19373
synced_at: 2026-01-12T15:56:37Z
```

# chore: Document Material for MkDocs Insiders limitations in CONTRIBUTING.md in more detail

---

_@corneliusroemer_

## Summary

It took me some time to figure out that as an outside contributor I can't reproduce the docs. This is because the Ruff docs use Material for MkDocs Insiders, which is closed source.

While CONTRIBUTING.md has terse comments to point outside contributors to a fallback, it does not go into much detail.

This PR adds a Note admonition to highlight to new contributors that they can't fully reproduce the docs locally, with a relevant link to the features that are missing from the OSS version.

I arrived here through a rabbit hole via https://github.com/astral-sh/ruff/issues/19362 and https://github.com/astral-sh/ruff/issues/19369

---

_Label `documentation` added by @ntBre on 2025-07-15 20:40_

---

_Label `internal` added by @ntBre on 2025-07-15 20:40_

---

_@MichaReiser approved on 2025-07-16 08:37_

Thank you

---

_Merged by @MichaReiser on 2025-07-16 08:42_

---

_Closed by @MichaReiser on 2025-07-16 08:42_

---
