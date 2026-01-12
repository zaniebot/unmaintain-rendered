```yaml
number: 17323
title: "Grouped, structured output for `uv lock --upgrade`"
type: issue
state: open
author: winwinashwin
labels:
  - enhancement
assignees: []
created_at: 2026-01-05T09:08:31Z
updated_at: 2026-01-06T17:13:09Z
url: https://github.com/astral-sh/uv/issues/17323
synced_at: 2026-01-12T16:02:48Z
```

# Grouped, structured output for `uv lock --upgrade`

---

_@winwinashwin_

### Summary

First off - huge fan of uv. It’s fast, predictable, and has quickly become my default tool for Python dependency management.

One small area where I think `uv lock --upgrade` could be even better is how upgrade results are presented.

**Current behavior**

When running:

```bash
uv lock --upgrade
```

the output lists dependency changes in a mostly linear fashion. While this is accurate, it becomes harder to reason about in larger projects or when many dependencies change at once.

**Proposed Improvement**

It would be incredibly helpful if the output were *grouped* and *categorized*, similar to the _grouped diagnostics view_ that Ruff provides (which I really love).

Specifically, grouping changes into categories such as:

- Major version upgrades
- Minor / patch upgrades
- Newly added dependencies
- Removed dependencies

### Example

**Why this helps**

- Makes risk assessment much faster (major vs minor bumps)
- Easier to review in CI logs
- Improves human readability for large dependency graphs


---

_Label `enhancement` added by @winwinashwin on 2026-01-05 09:08_

---

_Comment by @EliteTK on 2026-01-06 17:13_

Haven't formed an opinion on this topic yet but regarding distinguishing between major/minor:

Not everything uses semver. So the concept of "major minor" has different levels of significance depending on the dependency. Some dependency might use calver or something and now everything is considered a minor/patch upgrade until you hit new year's day.

---
