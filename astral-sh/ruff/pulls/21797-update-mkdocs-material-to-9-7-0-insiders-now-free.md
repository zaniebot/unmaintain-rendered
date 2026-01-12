```yaml
number: 21797
title: Update mkdocs-material to 9.7.0 (Insiders now free)
type: pull_request
state: merged
author: mahiro72
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/mkdocs-material-insiders-free
created_at: 2025-12-04T19:30:01Z
updated_at: 2025-12-05T07:53:09Z
url: https://github.com/astral-sh/ruff/pull/21797
synced_at: 2026-01-12T15:57:33Z
```

# Update mkdocs-material to 9.7.0 (Insiders now free)

---

_@mahiro72_

## Summary

Remove Material for MkDocs Insiders configuration since all Insiders features are now free in [mkdocs-material 9.7.0](https://squidfunk.github.io/mkdocs-material/blog/2025/11/11/insiders-now-free-for-everyone/).

Resolves #21528.

## Changes

- Update mkdocs-material to 9.7.0
- Delete `docs/requirements-insiders.txt` and `mkdocs.public.yml`
- Rename `mkdocs.insiders.yml` to `mkdocs.yml`
- Simplify CI workflows by removing SSH key conditionals
- Update CONTRIBUTING.md documentation
- Enable Renovate auto-updates for mkdocs-material

## Test Plan

Verified docs build locally:
```bash
uvx --with-requirements docs/requirements.txt -- mkdocs serve -f mkdocs.yml
```

---

_@MichaReiser approved on 2025-12-05 07:40_

Thank you

---

_Label `documentation` added by @MichaReiser on 2025-12-05 07:40_

---

_Merged by @MichaReiser on 2025-12-05 07:53_

---

_Closed by @MichaReiser on 2025-12-05 07:53_

---
