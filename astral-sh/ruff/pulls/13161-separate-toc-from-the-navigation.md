```yaml
number: 13161
title: Separate TOC from the navigation
type: pull_request
state: closed
author: dhruvmanila
labels:
  - documentation
assignees: []
base: main
head: dhruv/restructure-docs
created_at: 2024-08-30T13:02:52Z
updated_at: 2025-09-25T10:09:33Z
url: https://github.com/astral-sh/ruff/pull/13161
synced_at: 2026-01-10T17:40:28Z
```

# Separate TOC from the navigation

---

_Pull request opened by @dhruvmanila on 2024-08-30 13:02_

## Summary

This PR does the following:
1. Separate table of contents from the navigation
2. Group certain pages into a single header (like references for settings, rules, etc.)
3. Remove the "Ruff" header in the navigation similar to uv

For (2), the URL doesn't change. For example, even though the settings page is under "References", the url is still "/ruff/settings" instead of "/ruff/references/settings". Ideally, we'd want to move towards the latter URL but it will require creating redirects. I think we could either (a) keep the changes as it is or (b) postpone (2) to be done when we're restructuring the docs and / or adding versioned documentation. @zanieb would love to hear your thoughts here.

## Test Plan

`mkdocs serve --strict -f mkdocs.insiders.yml`, no warnings on generating the documentation site and validate the pages.


---

_Label `documentation` added by @dhruvmanila on 2024-08-30 13:02_

---

_Comment by @github-actions[bot] on 2024-08-30 13:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @dhruvmanila on 2024-09-05 10:46_

---

_Review requested from @zanieb by @dhruvmanila on 2024-09-05 10:46_

---

_Comment by @zanieb on 2024-12-10 15:11_

I'm supportive of any changes for consistency between the Ruff and uv docs. Especially w.r.t styling since it doesn't require a lot of work.

---

_@zanieb reviewed on 2024-12-10 15:12_

Should we be tackling the reorganization of the content separately?

---

_Comment by @dhruvmanila on 2025-09-25 10:09_

Ugh, sorry, I completely missed this, I'm going to close this for now as I haven't looked at it in a while.

---

_Closed by @dhruvmanila on 2025-09-25 10:09_

---
