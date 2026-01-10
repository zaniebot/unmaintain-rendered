```yaml
number: 1879
title: "Add support for callout syntax (#1875)"
type: pull_request
state: closed
author: leandrobbraga
labels:
  - documentation
assignees: []
base: main
head: mkdocs-callout
created_at: 2025-12-13T15:45:00Z
updated_at: 2025-12-14T09:33:13Z
url: https://github.com/astral-sh/ty/pull/1879
synced_at: 2026-01-10T02:34:11Z
```

# Add support for callout syntax (#1875)

---

_Pull request opened by @leandrobbraga on 2025-12-13 15:45_

This resolves the issue of Markdown callouts not rendering in MkDocs.

I used https://github.com/sondregronas/mkdocs-callouts, which is compatible with the syntax already being used in this specific part of the documentation.

## Before
<img width="1574" height="202" alt="image" src="https://github.com/user-attachments/assets/a98d23cf-d454-45f0-922f-38f5402ca998" />

## After
<img width="1550" height="294" alt="image" src="https://github.com/user-attachments/assets/712a0c5a-8b7e-48d7-b8e4-c308dd8af873" />

## Alternative Solution
An alternative solution would be to not add a new dependency and change the callout syntax in the original code. This was implemented in https://github.com/astral-sh/ruff/pull/21961.

```markdown
!!! warning "Deprecated"
    This option has been deprecated. Use `environment.root` instead.
```
<img width="1548" height="312" alt="image" src="https://github.com/user-attachments/assets/dc76808c-20dc-4cd9-8c70-5b8b226dee04" />


Closes #1875 


---

_Label `documentation` added by @AlexWaygood on 2025-12-13 15:48_

---

_Converted to draft by @leandrobbraga on 2025-12-13 15:51_

---

_Marked ready for review by @leandrobbraga on 2025-12-13 15:54_

---

_Converted to draft by @leandrobbraga on 2025-12-13 15:57_

---

_Renamed from "Add support for GitHub's style callout syntax (#1875)" to "Add support for callout syntax (#1875)" by @leandrobbraga on 2025-12-13 16:02_

---

_Marked ready for review by @leandrobbraga on 2025-12-13 16:02_

---

_Comment by @MichaReiser on 2025-12-13 17:07_

Thank you. Do you know how this plugin is different from mkdocs-github-admonitions-plugin which we use in ruff

---

_Comment by @leandrobbraga on 2025-12-13 17:23_

My understanding is that they serve similar purposes: mkdocs-callout converts from Obsidian’s format, and mkdocs-github-admonitions-plugin converts from GitHub’s format.

The issue is that the syntax used is `> [!WARN] "Deprecated"`, which is compatible with Obsidian’s format but not GitHub’s. GitHub uses `WARNING`, not `WARN`, and it does not support titles such as "Deprecated."

I believe the better solution is to use the admonitions format directly: `!!! warning "Deprecated"`. This format is already used throughout the codebase and does not introduce a new dependency.

Some examples: 
- https://github.com/astral-sh/ruff/blob/82a7598aa8dc5cd2dde18e4da6e0e24b48dc7392/docs/editors/index.md?plain=1#L24
- https://github.com/astral-sh/ruff/blob/82a7598aa8dc5cd2dde18e4da6e0e24b48dc7392/docs/editors/settings.md?plain=1#L21
```markdown
!!! note "Added in Ruff `0.9.8`"

    The **Inline JSON configuration** option was introduced in Ruff `0.9.8`.
```

I’ve opened this alternative solution at https://github.com/astral-sh/ruff/pull/21961.

---

_Closed by @MichaReiser on 2025-12-14 09:21_

---

_Branch deleted on 2025-12-14 09:33_

---
