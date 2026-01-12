```yaml
number: 383
title: "Fix missing '>' in HTML anchor tags"
type: pull_request
state: closed
author: Usul-Dev
labels: []
assignees: []
base: main
head: fix-html-syntax-cli-docs
created_at: 2025-05-14T14:08:05Z
updated_at: 2025-05-14T15:35:45Z
url: https://github.com/astral-sh/ty/pull/383
synced_at: 2026-01-12T15:54:27Z
```

# Fix missing '>' in HTML anchor tags

---

_@Usul-Dev_

## Summary

This PR fixes HTML syntax errors in the CLI documentation by adding missing closing angle brackets ('>') in anchor tags. While these missing brackets don't affect the rendered output, fixing them improves HTML validity and maintainability of the documentation.

Changes:
- Add missing '>' after href attributes in anchor tags in docs/reference/cli.md
- Affects documentation formatting only, no functional changes

## Test Plan

Verified the changes by:
1. Checking the rendered markdown locally to ensure the documentation appears identical
2. Validating the HTML syntax of the modified anchor tags
3. Confirming the links still work correctly in the documentation

The changes are purely syntactical and don't affect the functionality or appearance of the documentation.

---

_Comment by @MichaReiser on 2025-05-14 14:17_

Thanks for pointing out the issue. 

The docs are auto generated. Would you be interested in fixing the generation script here https://github.com/astral-sh/ruff/blob/main/crates/ruff_dev/src/generate_ty_cli_reference.rs instead?

---

_Closed by @MichaReiser on 2025-05-14 15:35_

---
